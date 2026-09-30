# Week 5 homework: cloud concepts, global infrastructure and security

Course: AWS Academy Cloud Foundations

| Task | What | Status | Evidence |
|------|------|--------|----------|
| 1.1 | Modules 1 to 4 and knowledge checks | Done | [Screenshot](#11-modules-1-to-4) |
| 1.2 | MFA on the root user | Done | [Screenshot](#12-mfa-on-the-root-user) |
| 1.3 | Budget alert | Done | [Screenshot](#13-budget-alert) |
| 2.1 to 2.4 | Written answers | Done | [Answers](#task-2-the-concepts-behind-the-clicks) |

---

## Task 1: AWS Academy and account hardening

### 1.1 Modules 1 to 4

I finished the knowledge check for each of Modules 1 to 4.

| Module | Score |
|--------|-------|
| 1 | 90/100 |
| 2 | 100/100 |
| 3 | 100/100 |
| 4 | 90/100 |

![Academy knowledge check grades](screenshots/01-academy-progress.png)

### 1.2 MFA on the root user

What I did:

1. Signed in as the root user and opened the security credentials page.
2. Added a virtual MFA device (an authenticator app on my phone) named `root-phone`.
3. Entered two consecutive codes to confirm it.

The IAM dashboard now shows "Root user has MFA" and 0 security recommendations. The account ID is redacted.

![IAM dashboard with root MFA enabled](screenshots/02-root-mfa.png)

### 1.3 Budget alert

I created a zero spend budget called `zero-spend-budget` from the template, with my own e-mail as the recipient. The template sets a $1.00 budget and notifies me when spend goes above $0.01.

![Created budget](screenshots/03-budget.png)

**What triggers the alert e-mail, and how long after the spend does it arrive?**

> AWS sends the e-mail when its billing system records actual spend above the $0.01 threshold. It is not instant. Billing data is processed in batches and Budgets reads it only a few times a day, so the e-mail can arrive several hours to about a day after the spend happened. The console itself says a new budget can take up to 24 hours to populate spend data.
>
> A resource can keep running and accumulating charges during that gap, so the alert is a safety net and not a real-time stop.

---

## Task 2: The concepts behind the clicks

### 2.1 Shared responsibility misunderstood

> A small company stores customer invoices (names, addresses, partial card details) in an Amazon S3 bucket. The developer assumes AWS "secures S3", so the data must already be private. To let a reporting tool read the files, they switch off S3 Block Public Access and give the bucket a public-read ACL.
>
> AWS is responsible for security of the cloud: the hardware, the data centers and S3's durability. The customer is responsible for security in the cloud, which includes bucket permissions and access policies.
>
> A few days later a researcher finds the open bucket in a public scan and downloads thousands of invoices. The result is a reportable data breach, possible GDPR fines and lost customer trust, even though AWS did nothing wrong. The fix is to keep Block Public Access on and give the tool an IAM role with least privilege.

### 2.2 Capital expense versus operating expense

> A server bought outright is a capital expense. The price is paid once, so nothing can surprise you afterwards.
>
> Cloud is an operating expense that grows with consumption, so a forgotten instance or a runaway job keeps charging with no built-in ceiling. The budget alert takes the place of the limit that a purchase price used to provide.

### 2.3 Regions and Availability Zones

> A Region contains several Availability Zones, which are separate data centers with their own power, cooling and networking, joined by low-latency links. Because they are separate, a fire, power cut or network fault in one zone does not take down the others.
>
> An architect who spreads a system across zones, for example web servers behind a load balancer and a database with a standby replica, gets high availability. If one zone fails, traffic moves to the healthy ones with little or no downtime, and the low latency between zones still allows synchronous replication.

### 2.4 Users versus roles

> A role gives temporary credentials that expire on their own, usually within hours, and AWS delivers them to the workload, so nobody has to store or rotate a secret.
>
> An IAM user's access keys are long term and stay valid until someone rotates or deletes them. Those keys often end up in code, config files or public Git repositories, and anyone who finds them can use them from anywhere for as long as they stay valid. A leaked role credential stops working when it expires, so the window for abuse is much shorter.
