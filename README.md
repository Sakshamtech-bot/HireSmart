# SkillBridge
## Beyond Job Searching — Towards Career Readiness
**Tagline:** Connecting Skills with Opportunities.

SkillBridge is a proposed full-stack career-readiness platform that helps students discover internships and entry-level opportunities, understand how their skills compare with role requirements, identify learning gaps, and present practical evidence of their abilities.

> **Status note:** This document describes the planned project. Mark a feature as completed only after it has been implemented and tested.

## 1. Problem and proposed solution
Students often find opportunities but still need to understand whether their current skills fit a role, which skills are missing, and how to demonstrate practical ability. SkillBridge connects opportunity discovery with skill development and evidence in one workflow. It is not intended to replace established recruitment networks.

## 2. Main differentiators
### Explainable Smart Skill Matching
Compare student skills with opportunity requirements. Display matched skills, missing skills and the score calculation.

**Example:** Required skills: Java, Spring Boot, MySQL, REST API, Git. Student skills: Java, MySQL, REST API. Match = 3/5 × 100 = **60%**. Missing: Spring Boot and Git. This is an indicative score, not a hiring decision or eligibility guarantee.

### Skill Gap Analyzer
Identify missing skills and suggest learning topics and practical milestones. Initial recommendations will be curated/rule-based, not claimed as AI-generated.

### Skill Passport
A structured profile containing education, interests, skills, proficiency levels, GitHub projects, live project links, certificates and resume. Evidence labels should distinguish self-declared, submitted and reviewed items.

### Opportunity and Application Management
Recruiters publish and manage opportunities. Students search, filter, apply and track application status. Recruiters review applicants and update statuses.

### Platform Comparison and Career Insights
An educational comparison page will describe publicly documented platform focus and selected capabilities for LinkedIn, Handshake and Indeed, alongside SkillBridge's intended focus. This is not a claim that competitors lack these features. Add source URLs and last-reviewed dates; use **Not verified** when information is uncertain.

Official links:
- LinkedIn: https://www.linkedin.com/
- Handshake: https://joinhandshake.com/
- Indeed: https://www.indeed.com/

## 3. User roles
| Role | Responsibilities |
|---|---|
| Student | Profile, skills, evidence, resume, opportunity discovery, matching and applications |
| Recruiter | Company profile, opportunity posting, required skills and applicant review |
| Admin | User management, listing moderation and comparison content maintenance |

Enforce permissions in backend APIs, not only by hiding frontend controls.

## 4. Technology stack
| Layer | Technology | Purpose |
|---|---|---|
| Frontend | HTML5, CSS3, JavaScript, Bootstrap 5 | Responsive UI |
| Backend | Java, Spring Boot | REST APIs and business logic |
| Persistence | Spring Data JPA, Hibernate | ORM and database access |
| Database | MySQL | Relational storage |
| Security | Spring Security, BCrypt | Authentication and authorization |
| Build/testing | Maven, JUnit, Mockito, Postman | Build and testing |
| Version control | Git, GitHub | Source management |

MySQL fits the connected data model: users, profiles, skills, opportunities and applications require relationships, joins and constraints.

## 5. High-level architecture
```text
Student / Recruiter / Admin
          |
HTML + CSS + JavaScript + Bootstrap
          | HTTP / JSON (REST)
          v
Java + Spring Boot
  Controllers -> Services -> Repositories
  Security + Matching + Skill Gap Logic
          | Spring Data JPA / Hibernate
          v
        MySQL
```
Controller receives requests; Service applies business rules; Repository performs persistence; Entity maps domain objects; DTO defines request/response data; Security checks identity and permissions.

## 6. Proposed database tables
| Table | Purpose |
|---|---|
| users | Account identity, email, password hash and role |
| student_profiles | Education, interests and goals |
| recruiter_profiles | Recruiter and company details |
| skills | Shared skill catalogue |
| student_skills | Student skills and proficiency |
| skill_evidence | Project links, certificates and evidence status |
| opportunities | Internship, project and job listings |
| opportunity_skills | Skills required by opportunities |
| applications | Student applications and current status |
| application_status_history | Status timeline |
| learning_roadmaps / roadmap_items | Learning plans and milestones |
| competitor_platforms | Platform names and descriptions |
| platform_features | Comparison feature catalogue |
| platform_feature_comparison | Platform-specific notes |
| comparison_sources | Source URLs and review dates |

## 7. Proposed REST API plan
| Method | Endpoint | Purpose |
|---|---|---|
| POST | /api/auth/register | Register |
| POST | /api/auth/login | Login |
| GET/PUT | /api/students/me | View/update profile |
| POST | /api/students/me/skills | Add skill |
| POST | /api/students/me/evidence | Submit evidence |
| GET | /api/opportunities | Browse opportunities |
| POST | /api/opportunities | Recruiter creates opportunity |
| GET | /api/matching/recommendations | Matching results |
| GET | /api/matching/gaps/{opportunityId} | Missing skills |
| POST | /api/applications | Apply |
| GET | /api/applications/me | Track applications |
| GET | /api/comparison/matrix | Platform comparison |

These are proposed contracts; update them to match the implemented routes.

## 8. Competitor comparison approach
- **LinkedIn:** professional identity, networking and job discovery.
- **Handshake:** student and early-career recruiting and employer connections.
- **Indeed:** broad job discovery and application-related services.
- **SkillBridge:** intended focus on explainable skill matching, gap identification and a structured Skill Passport.

Compare only specific, sourced claims. Capabilities may vary by country, account and plan. Include review dates and distinguish Available, Limited/varies and Not verified.

## 9. Security and privacy
- Hash passwords with BCrypt; never store plain-text passwords.
- Enforce authorization on every protected backend endpoint.
- Validate uploaded file type and size; protect resume files.
- Never commit credentials, database passwords or secret keys.
- Collect only necessary information and avoid exposing private files.
- Record application status changes without leaking sensitive data.

## 10. Testing plan
1. Reject duplicate email registration.
2. Confirm passwords are hashed.
3. Confirm students cannot access recruiter-only APIs.
4. Confirm recruiters cannot modify another recruiter's listings.
5. Verify match scores with known test data.
6. Verify missing-skill output.
7. Prevent duplicate applications.
8. Reject invalid or oversized uploads.
9. Record application status history.
10. Require a source and review date for comparison entries.

## 11. Eight-week roadmap
| Week | Planned work |
|---|---|
| 1 | Requirements, wireframes, ER diagram and repository setup |
| 2 | Spring Boot, MySQL, JPA, registration and login |
| 3 | Profiles and role-based security |
| 4 | Opportunity posting, search and filters |
| 5 | Matching engine and skill-gap calculation |
| 6 | Skill Passport, evidence and learning milestones |
| 7 | Applications, recruiter review, admin and platform comparison |
| 8 | Testing, fixes, deployment, documentation and demo |

## 12. Future scope
NLP-based semantic matching, richer learning recommendations, evidence-review workflows, notifications, analytics and external integrations are future enhancements until implemented and evaluated. The first version should be described honestly as rule-based matching.

## 13. Suggested repository structure
```text
SkillBridge/
├── frontend/
│   ├── index.html
│   ├── login.html
│   ├── register.html
│   ├── student-dashboard.html
│   ├── recruiter-dashboard.html
│   ├── opportunities.html
│   ├── skill-passport.html
│   ├── skill-gap.html
│   ├── platform-comparison.html
│   ├── css/
│   └── js/
├── backend/src/main/java/com/skillbridge/
│   ├── config/  ├── controller/  ├── dto/
│   ├── entity/  ├── repository/  ├── security/  └── service/
├── backend/src/main/resources/
├── backend/pom.xml
├── database/schema.sql
├── docs/
└── README.md
```

## 14. Short presentation script
“SkillBridge is a career-readiness platform that connects students with relevant opportunities. Our workflow explains how a student's skills compare with an opportunity, identifies missing skills, and lets students present practical evidence through a Skill Passport. Recruiters can publish opportunities and review applications. We also provide a sourced comparison page to help students understand established career platforms. Our first matching engine is rule-based and transparent, with advanced NLP recommendations planned as future work.”

## 15. Submission checklist
- Replace planned features with actual implementation status.
- Confirm API routes against code.
- Add screenshots, final ER diagram and SQL schema.
- Test role permissions and matching calculations.
- Review competitor claims and source dates.
- Remove credentials and local environment files from Git.
- Add confirmed team, supervisor and institution details.
- Test setup instructions on a clean machine.
