---
layout: default
title: JARVIS Career Agent Privacy Policy
permalink: /privacy/
---

# JARVIS Career Agent Privacy Policy

**Effective date:** September 30, 2026

**Release notice:** The resume storage, analytics, Drive save, and archive changes
below describe the reviewed release candidate. They require approval, schema
provisioning, and deployment before becoming available in the live application.

**Operator:** David Russell Meredith — The Meredith Collective

**Location:** DTC, Colorado, United States

**Privacy contact:** [administrative-agent@tmcsolutions-org.net](mailto:administrative-agent@tmcsolutions-org.net)

## Purpose and current availability

JARVIS Career Agent is a private, single-owner career-management application intended to help its owner find steady employment and income. It organizes career evidence, job postings, resumes, applications, and follow-up activity. It is not offered for public registration or use by other people.

The application is still being provisioned. LinkedIn sign-in, Google Gmail, Calendar, and Drive integration, AI-assisted resume tailoring, and synchronization of public job postings are intended for the initial release. This notice describes implemented data paths and clearly identifies planned functionality; publishing it does not mean the application or every integration is operational. Databricks application permissions, not LinkedIn sign-in, are intended to restrict access to the owner.

## Career information

The application processes owner-supplied employment history, education, certifications, skills, and other career evidence. Job-tracking records can contain company and role names, job URLs, locations, compensation ranges, application statuses, dates, and private notes.

The configured database stores postings, application records, resume-version metadata, match scores, gap analyses, application activity, and job-posting synchronization records. The release candidate also stores generated resume text privately in Lakebase so the owner can reopen and download a draft. Application list responses omit this text. A separate, explicit Drive save can store a text file and its file identifier after Google authorization is configured. These records support the owner's career preparation and application tracking, not advertising or data brokerage. Legacy drafts with no stored text require re-tailoring.

## Job-posting sources

Job-posting synchronization is implemented and tested but, as of this version, is not yet enabled in the deployed application. When the owner enables it, the application requests public job listings from three sources: AI Dev Jobs (`aidevboard.com`), The Muse (`www.themuse.com`), and USAJOBS (`data.usajobs.gov`, operated by the U.S. Office of Personnel Management). It contacts no other job board. Indeed and ZipRecruiter appear in its configuration as disabled and are never called.

A synchronization runs when the owner starts one. A scheduled hourly run is planned and not yet set up. Requests originate from the deployed application or from the owner's computer, so each source receives that system's IP address and ordinary request metadata. A reachability check sends each source one request with no key and no search criteria.

Each synchronization request carries search criteria only. Depending on the source, these are job-title keywords, a metropolitan area with a radius, place or state names, a remote-work preference, a seniority level, and job categories. No career evidence, resume text, application record, or private note is sent to a job source. USAJOBS requires an API key and the email address registered with it, and the application sends that email address as the `User-Agent` header on every USAJOBS search request. AI Dev Jobs and The Muse work without a key; the application sends either of them an API key only when the owner has configured one.

From each listing the application stores the title, employer, location, work arrangement, seniority, pay range, category, employment type, description as plain text, listing and application links, and posting and closing dates. Descriptions are third-party text and can name people or include contact details the employer chose to publish. For USAJOBS only, the application also keeps the source's original record, which can include the hiring agency's published contact email address and telephone number. It does not keep the original record from AI Dev Jobs or The Muse.

The application records each synchronization run: the source, start and finish times, outcome, item counts, and a short error description. The application's own request logs name the host, path, status, and timing; they do not include query strings, keys, or the registered email address. This is not a guarantee about provider diagnostic logs.

Synchronized postings are updated when a source changes them and are marked as likely closed after their closing date or when no synchronization has seen them for 14 days. The application does not delete them automatically; they remain until the operator removes them. Stored descriptions are shown to the owner and, when the owner requests tailoring for a posting, are sent to Anthropic as described below.

Each source's own terms and privacy policy govern how it handles these requests.

## LinkedIn sign-in

LinkedIn sign-in is included in the launch plan but remains subject to setup and live verification. When the owner chooses to connect, LinkedIn handles authentication and consent. The application requests `openid`, `profile`, and `email`. It does not collect the owner's LinkedIn password.

The sign-in callback exchanges an authorization code for an access token and requests LinkedIn's user-information response. That response may include a member identifier, name, profile-picture URL, email address, and related identity claims, depending on what LinkedIn returns.

The implemented callback retains only the member identifier and name in a signed browser session cookie to identify the connected account. It processes the token and identity response in server memory and does not intentionally persist the LinkedIn access token, email, or picture in the application database or cookie. This is not a guarantee about provider diagnostic logs.

Identity is retrieved during sign-in, not by scheduled background refresh. This integration does not access LinkedIn messages, connections, or job-search history. The LinkedIn session identity is not an input to resume tailoring. Separately supplied career evidence is distinct from identity information retrieved through this sign-in integration.

## Google integrations

Gmail, Calendar, and Drive are core requirements, not merely optional future enhancements. The app renews access tokens server-side before each synchronization. The release candidate implements bounded retrieval and an explicit Drive writer. Ongoing access still depends on a valid OAuth client, fresh owner consent, and live verification; the current Google configuration requires reconnection before use. Tokens are not stored in browser cookies or sent to Anthropic.

When configured and invoked, the existing Gmail/Calendar synchronization code requests recent Gmail message metadata and upcoming primary-calendar events. It can process message identifiers, sender headers, subjects, timestamps, snippets returned by Gmail, event titles, and attendee email addresses to match activity to applications. Matching occurs after retrieval; the initial retrieval is not restricted to known recruiters. Matched subjects or event titles, source identifiers, and timestamps can be stored as application activity in Lakebase.

The reviewed Gmail path does not request full message bodies or attachments, although the intended `gmail.readonly` authorization permits broader reading than these requests. Calendar access is intended to be read-only. These paths do not send emails or change calendar events, and their implemented matching logic does not call an AI model.

After separate authorization, the release candidate's Save to Drive button sends the selected generated resume as a plain-text file to the owner's configured private folder. It requests the limited `drive.file` scope for files created by or explicitly shared with the OAuth app; an existing folder identifier alone does not grant access. This transfers the resume, including its verified contact header, to Google. The app stores the returned file identifier and reuses it on repeated saves. It does not change sharing permissions, send the file to an employer, or delete Drive files automatically. If Google creates a file but the database cannot record its identifier, the owner must reconcile the file before retrying. The Lakebase draft and download remain available if Drive fails.

Any enabled use of Google API data, including transfers, must follow the [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy), including its Limited Use requirements. Google data will be used for the disclosed owner-facing features, not advertising, data brokerage, or training generalized AI models. Additional access or use requires policy review and any required consent before activation. This is an operating commitment, not a claim of Google approval or a completed security assessment.

## AI-assisted resume tailoring

When the owner requests tailoring and the feature is configured, selected posting fields and career evidence are sent to Anthropic's API. The posting fields include the stored description, limited to 12,000 characters, whether the owner entered it or a job source supplied it. The evidence includes relevant experience, education, certifications, and skills. Anthropic returns resume text, a match score, keyword analysis, and suggested certifications. These inputs can contain personal information such as employment history.

The implemented tailoring path does not read `applications.notes`, LinkedIn session identity, or Gmail/Calendar activity as prompt inputs. This field separation does not remove sensitive information independently placed in career evidence or other AI inputs. The owner should submit only information they are willing and authorized to provide for AI processing.

Outputs require human review. The reviewed tailoring route does not automatically submit applications or send resumes to employers. Anthropic's applicable service terms and account settings govern its handling of API inputs and outputs; this policy does not promise zero provider retention or a particular processing country. No assertion is made that the operator has a special zero-retention agreement.

The release candidate adds the verified name and contact header locally after the
model returns a draft. That header is not an input to the tailoring model. Other
personal information independently present in career evidence can still be sent
as described above.

## AI-assisted application analytics

When the owner uses Ask Keener in the release candidate, the question and an
approved database schema are sent to Anthropic to produce a read-only query. The
server checks effective database permissions before this call, runs the query
under a restricted PostgreSQL role with a timeout and row limit, and sends only
approved operational or aggregate result fields to Anthropic for explanation.
These fields can include employer/role names, application stages and dates,
counts, and evidence-match scores. Private notes, activity summaries, resume text,
original job-source records, and archive contents are excluded from this path.
The owner's question is an AI input; do not include sensitive information that
the owner does not intend to send to Anthropic. Answers require human review.

## Providers and access

- **Databricks and its infrastructure providers:** hosting, workspace authentication, Lakebase database storage, and operational services. Authorized platform administrators may have access under the configured permissions.
- **LinkedIn:** authentication, consent, and the identity response described above.
- **Google:** authorized Gmail, Calendar, and Drive operations when enabled.
- **Anthropic:** the AI processing described above when requested and configured.
- **AI Dev Jobs, The Muse, and USAJOBS (U.S. Office of Personnel Management):** public job listings returned for the search requests described above, when job-posting synchronization is enabled. USAJOBS also receives the owner's registered email address with each search request.
- **Cloudflare:** hosting this public documentation site on Cloudflare Workers. Cloudflare processes visitors' IP addresses and request metadata to deliver and protect the site, as described in its [privacy policy](https://www.cloudflare.com/privacypolicy/). This site contains public policy documentation, not private career records or application credentials.
- **GitHub:** hosting the documentation source repository.

The operator does not sell personal information, use it for advertising, or provide it to data brokers. No additional provider is currently designated. Adding providers requires review of their data access, an updated notice, owner approval, and any required consent; this policy is not blanket authorization for unspecified future sharing.

Provider processing locations, service logs, and backups are governed by applicable provider terms and settings. The owner-managed career-record schedule below is not a guarantee that provider copies have the same lifetime or remain in one country.

## Cookies and logs

The application uses a signed session cookie for LinkedIn OAuth state and the connected identifier and name. A signature protects against undetected modification; it does not encrypt the cookie's contents. Clearing application cookies removes that browser's stored session but does not revoke provider authorization or delete database records. Browser session restoration can preserve session cookies.

The release candidate expires LinkedIn sign-in state after 10 minutes and consumes
it before exchanging a code. Its signed identity cookie has an eight-hour lifetime
and uses Secure, HttpOnly, and SameSite=Lax in production. Databricks authentication
has separate platform-managed cookies and remains the access boundary.

Databricks and authentication providers can use their own cookies and process connection metadata, requests, errors, and identifiers. Cookie configuration, log content, and provider retention must be checked before deployment; no fixed deletion interval for logs or backups is promised by this notice. Additional Unity Catalog telemetry export has not been verified as enabled. This documentation site does not add advertising trackers or analytics scripts.

## Retention, archives, and deletion

The owner has adopted these requirements for owner-managed career records:

- **Application records:** remain active while an application is open and for one year after application closure, then are archived and compressed.
- **Archives:** remain until the owner explicitly approves deletion, without a fixed automatic expiry. Archives remain personal information; compression is not deletion or anonymization.
- **Master career evidence:** employment history, education, and certifications remain active indefinitely, separate from the one-year application-record schedule.

The release candidate tracks closure when an application enters Rejected or
Withdrawn. Switching between those stages preserves its closure date; reopening
clears it. A legacy record with an unknown closure date is skipped until the owner
supplies a verified date. The owner explicitly runs Archive eligible records;
each run handles up to 100 records at or beyond one calendar year after closure.
A February 29 closure reaches its anniversary on February 28 in the following
non-leap year. Archiving removes records from the active pipeline, stores a
compressed private snapshot and checksum, and preserves the original application,
resume, notes, and events. Restoring checks snapshot integrity and reopens the
application as Applied; historical snapshots remain. There is no background
archive schedule or deletion endpoint. Records and snapshots remain until the
operator takes an approved action. Compression does not remove provider data or
override deletion obligations.

David Russell Meredith (Russ Meredith / Russypher) is responsible for administration and privacy requests. J.A.R.V.I.S. may assist with authorized administration, but every deletion by J.A.R.V.I.S. requires the owner's explicit permission. Permission to archive or compress is not permission to destroy source records. J.A.R.V.I.S. is not a separate legal operator, and this policy itself grants no technical privileges.

This career-record schedule does not authorize keeping provider-sourced data or credentials contrary to applicable law or provider requirements. Applicable deletion obligations take precedence, including LinkedIn's requirements for deletion of API data on request, account closure, or other specified events. The owner remains responsible for approving and completing required actions promptly.

## Choices and requests

Contact [administrative-agent@tmcsolutions-org.net](mailto:administrative-agent@tmcsolutions-org.net) to ask about access, correction, export, or deletion. Identify the relevant records without sending passwords, API keys, or tokens. The owner handles requests, may seek proportionate confirmation of identity, and authorizes any necessary administrative actions.

LinkedIn and Google authorization can be withdrawn using the respective account's connected-application controls. Revocation stops future authorized access but does not automatically delete already-stored records or application cookies. Request removal separately. The reviewed app does not yet provide a dedicated account-deletion or LinkedIn disconnect/logout endpoint.

The owner can choose not to connect a provider or invoke AI features. Provider-controlled records and backups may require separate provider procedures; instantaneous deletion of every copy is not promised. Mandatory legal or provider obligations continue to apply.

## Security and policy maintenance

The intended deployment uses HTTPS, owner-restricted access, protected credentials, and a dedicated database role. Some controls still require deployment verification; no system or policy can guarantee absolute security. The application is not intended for children or a public user population.

This policy is maintained in the [public documentation repository](https://github.com/TMC-The-Meredith-Collective/career-agent-docs). The operator will review and update it before materially changing integrations, data uses, or the audience, and obtain additional consent where required. Applications and integration registrations using this policy should link to this current version. Version history records published changes. Before allowing other users, the operator will separately review access controls, privacy obligations, and change notifications.

Privacy questions: [administrative-agent@tmcsolutions-org.net](mailto:administrative-agent@tmcsolutions-org.net).
