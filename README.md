# Student Organization IT Solution
## TLDR
An IT-solution to organize student organizations that regularly intake new members, graduate alumni, and transition executive boards often. 

## Existing Greek-oriented Solutions
These solutions below are tuned for Greek-lettered chapters (fraternities/sororites), encompassing social, professional, cultural and other types of Greek chatpers.

- [Anthology Engage](https://www.anthology.com/products/lifecycle-engagement/enrollment-and-retention/anthology-engage?utm_source=chatgpt.com)
  - Formerly named Campus Labs Engage, and before that OrgSync. 
  - Provided by [University of Oregon](https://uoregon.campuslabs.com/engage/) for all university-affiliated student organizations.
- [re:Members CRM](https://www.remembers.com/fraternal/crm/)
  - re:Members provide a handful of Greek-oriented products.
  - CRM stands for Customer Relationship Management, however the functions of CRM software aligns closely with all Greek IT solutions.  
- [OmegaFi](https://www.omegafi.com/solutions/omegaone) 
  - OmegaFi also provides a handful of Greek-oriented products.

# Software Development Implementation

## Nomenclature
- A **member** is an individual within an organization.
  - Some organizations may designate between "new" members and active members.
  - Most organizations consider alumni to be inactives upon graduation.
- An **officer** is any member with heightened responsbility/obligation to the organization.
  - Some common examples are President, Vice-President, Secretary, Treasurer.
- A **committee** is a subgroup of an organization's members that a subset of the organization's tasks are delegated to.
  - Some common examples are Civil Impact, Member Engagement, Judicial. 
- A **document** is anything that can be considered anything/everything related to an organization's official operations.
  - Most often generated by officers, within this platform or uploaded.
  - Stored on this platform for future reference.
  - May be distributed out to the rest of the organization, or subgroups.
  - Some common examples are bylaws, meeting notes, strategic plans, week/term/annual reports, announcements, congratulations, news updates, etc.
- An **event** is any official event/deadline/due date related to the organization.
  - Events should always be represented on a calendar, visible to every member, although with different content based on permissions.
- A **tag** can be applied to members, documents, and events.
  - Tags are often a time or a topic.
  - Tags may automatically organize documents/members/events into filtered groups.
  - Common Examples of Tags are: Fall 2024, Judicial, Philanthropic, Treasury
- **Forms** are the function meant to gather info from members, can be sent out by officers.
  - Forms may be open-ended or time-constained
  - Submission may be single, multiple, or editable.
  - Some common examples are: Service Hours Submission, Contact Info, Extracurriculars, Profile Picture, Judicial Complaint.
- **"blame"** (more commonly known as "Edit History") refers to when/who/how data was created/modified/deleted.
  - WHEN: the timestamped of the change.
  - WHO: the member that made the change.
  - HOW: the ["diff"]() representing the change.

## Structures in Plain-English
### Roster Management
The roster structure is crucial for authentication and authorization
- Authenticate our users through traditional username & password.
  - 2FA may be required for officers as their visibility to sensitive documents and abilities are more promiment.
- Authorization for Read/Write on **Documents**, **Calendars**, and **Events** is controlled, and limited to those necessary.
  - In addition to authorization, every change must have a "blame" (the member that made a change) recorded for retrospective reference & accountability.
- Control of membership is delegated to an officer; 
  - When a new member joins, they need to be onboarded.
  - When an active member departs, they need to be archived, or transferred to alumni organization.

Roster **Fields** may contain all sorts of member information, populated either by **officers**, or by a member themselves via a **Form**. 

Each **roster field** should have its own read/write permissions and edit history.

### Officers
An **officer** is any member with heightened responsbility/obligation to the organization. Officers are typically elected by the members of an organization, or appointed by other officers/committees within an organization.
> Some common examples are President, Vice-President, Secretary, Treasurer, etc.

**Officer transition** is a crucial aspect of this platform, from one board to the next, the transfer of power must be smooth.

### Committees
A **committee** is a subgroup of an organization's members that a subset of the organization's operations are delegated to.

> Common examples; Civil Impact, Member Engagement, Judicial, etc. 

A member may be in 0,1, or more than 1 committtees.

### Documents 
Documents store anything/everything related to an organization's official operations.

In most organizations, most documents are generated by officers.

Most documents should be kept for future reference and/or distributed to the members (or some subset of the members).

> Common examples are; meeting notes, strategic plans, week/term/annual reports, announcements, etc.

Documents can contain *intelligent references* to other documents, specific members, or events.

**Collections** of documents can be formed based on a filter of document tags.

### Events 
An organization's calendar should contain all official events/deadlines/due dates.

Different events may have different read permissions based on necessity and sensitivity.

An RSVP function may be attached to an event.


### Tags
Tags typically refer to a time or a topic.

Tags can be applied to members, documents, and events.

> Examples of Tags are Fall 2024, Judicial, Philanthropic, President, etc.

Tags may automatically place documents into collections, members into committees,give events formatting.

## Structures in CS
1. **Fields**
   - Each member has a UUID and their own collection of fields. 
   - Fields may be optional, or required based on criterion (or no critera, i.e. all members).
   - Examples of fields are name, profile picture, time joined.
   - Additional fields may be short-text, long-text, numeric, time, or file-upload formats.
   - Fields are editable by officers via **Roster Management** or supplied by the member via the **Form** function of this platform.
   - Fields can have tags applied to them.
   - Each field has its own read/write permissions.
2. **Tags**
   - Each tag has an ID and Name. Tags are typically a time period or a topic.
     - IDs: (does not change once created, not user-facing)
       - Term/2024/Fall
       - Committee/Judicial
       - Officer/President
       - Recruitment/Fall 2024 Cycle
     - Names: (user-facing)
       - Fall 2024 Term
       - Judicial Committee
       - Presidential
       - Member since Fall 2024
   - Allows for filtration of members/fields/documents/events by applying a tag-based filter.
3. **Officers**
   - An officer is assigned a role with a heightened authority (and responsbility) within the organization, typically by election or appointment.
   - Different roles have different permissions, and levels of authority.
   - Typical authority structures have a President at the tip of the hierarchy, with the ability to designate the rest of the officer roles.
4. **Committees**
   - A committee itself is a tag, and can be applied to members/fields/documents/events.
   - If a member is tagged with a committee, that member becomes privy to the documents/fields/events associated with said committee.
5. **Documents**
   - Documents are composed of rich-text data, with the ability to embed multimedia.
   - A document has a creator, a member (likely an officer)
   - A document has an owner, either the Officer role, or a Committee.
     - This is to enforce that documents should not be lost to an officer transition.
   - Documents should rarely be deleted, record-keeping is important. Archiving with restricted permissions is a better methodology, if necessary.
6. **Events**
   - Events have date, time, name, description, and can be tagged.
   - Events may be designated optional or required based on some criterion (or no critera, i.e. all members).
   - The presiding officer of an event may choose to record member attendance, especially at events deemed "mandatory".