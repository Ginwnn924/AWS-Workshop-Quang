---
title: "Sharing and Feedback"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 7. </b> "
---



### Overall Evaluation

**1. Working Environment**

The FCAJ environment is friendly and open. Interns and mentors are willing to help on group chat when AWS errors occur or workshop labs get stuck — for example, when I couldn't SSH into EC2 due to a wrong security group rule, a peer pointed me to the Reachability Analyzer within minutes. The learning space (online/offline depending on the session) helps focus on VPC Endpoint labs and personal projects. I would appreciate more **lightning talks** (5 minutes per person on their project) to learn from each other beyond mentor sessions. A shared **FAQ channel** by week (CloudFormation errors, IAM policy denies) would also reduce repeated questions.

**2. Support from Mentor / Team Admin**

Mentors guide clearly when I'm unfamiliar with Bedrock model access or RDS security group configuration. Instead of giving the answer immediately, mentors suggest which AWS docs to read — helping me debug independently and remember longer. The admin team supports procedures, report deadlines, and Hugo templates promptly. I especially value being allowed to **trial-and-error** on a real internship AWS account with budget alerts enabled. When I accidentally left a NAT Gateway running over the weekend, the budget alert reminder was a practical lesson in cloud cost awareness.

**3. Relevance of Work to Academic Major**

The Legal RAG Chatbot project fits IT majors well: backend (Python/FastAPI), AI (RAG, LLM), cloud (AWS), and technical writing. The S3/VPC workshop adds networking knowledge — at university I rarely practiced VPC Endpoints or hybrid DNS. The program extends classroom learning in the right direction for students aiming at cloud or AI engineering roles.

**4. Learning & Skill Development Opportunities**

During the internship I learned many new skills:

* Deploying and configuring AWS services (EC2, S3, RDS, Lambda, Cognito, Bedrock...).
* Designing end-to-end RAG pipelines — not just calling a ready-made chatbot API.
* Basic Infrastructure as Code with CloudFormation (`foundation.yaml` stack).
* Soft skills: weekly worklog reporting, product demo, receiving and applying feedback.

[Cloud Journey](https://cloudjourney.awsstudygroup.com/) and internal workshop materials were essential references throughout all 8 weeks. Lab links from AWS Study Group (IAM, VPC, RDS, S3, Cognito, Lambda) saved significant setup time compared to reading raw AWS docs alone.

**5. Company Culture & Team Spirit**

FCAJ culture is positive: everyone respects each other despite different schools or project topics. Near workshop cleanup deadlines, peers remind each other to delete resources (RDS, EC2, NAT Gateway) to avoid unexpected charges — a very practical **cloud cost awareness** spirit. During Week 3 proposal writing, our group reviewed each other's architecture diagrams and caught missing VPC Endpoint routes before deployment. I felt treated as a team member, not just someone "going through the motions."

**6. Internship Policies / Benefits**

The program provides lab AWS accounts, mentors, and a clear 8-week roadmap. Schedule flexibility helps when university coursework deadlines overlap. Access to the AWS Study Group community is a major plus for post-internship career development. The Hugo report template and bilingual structure (VI/EN) also helped me practice technical documentation — useful for portfolio and future job applications.

### Additional Questions

**What did you find most satisfying during your internship?**

Building a **real project** (legal RAG chatbot on AWS) instead of only making theory slides. When the demo correctly answered a question about land law with source chunk citations, I clearly saw the value of 8 weeks of effort. Deploying the Hugo report site locally and seeing the full worklog + proposal + workshop come together also felt like a tangible portfolio milestone.

**What do you think the program should improve for future interns?**

* Add fixed **office hours** for Bedrock/AI topics — many interns get stuck on model access and service quotas.
* Automated weekly **cost cleanup checklist** (RDS, EC2, NAT Gateway, Elastic IP) — prevents accidental charges.
* Proposal/worklog templates with more **AI/GenAI project examples** alongside existing IoT/serverless samples.
* Short video walkthroughs for Hugo setup and GitHub Pages deploy — reduces friction in the final reporting week.

**If recommending to a friend, would you suggest they join FCAJ? Why or why not?**

**Yes.** Reasons: clear hands-on AWS roadmap, mentor support, Study Group community, and opportunity to build a portfolio project for CV. Suitable for year 3–4 students pursuing cloud or AI engineering paths. The 8-week structure is short enough to fit a summer internship but long enough to deploy something demo-ready.

### Suggestions & Expectations

* Organize a **mini hackathon** at the end (1–2 days) for cross-team demos and peer review.
* Expand Vietnamese documentation for Amazon Bedrock and pgvector on RDS — most AWS docs are in English and slow down beginners.
* I hope to **continue contributing** to open-source workshop repos or AWS Study Group blogs after the internship ends.
* Other feedback: A Slack/Discord channel archiving Q&A by week would make it easier to search past CloudFormation, IAM, and Bedrock issues instead of asking the same questions repeatedly.

---

Thank you to the FCAJ team, mentors, and AWS Study Group for creating a valuable learning environment throughout my internship.
