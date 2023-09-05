# SVX 1.2.1 Release Notes

**Software Release Date**: Sept 4, 2023

**Summary**:

The following is a summary of the key features and enhancements:

* **Backend Pagination, Sorting and Searching**: This upgrade enhances the user experience by making data interaction more intuitive and consistent.
* **Email Notifications**: Significant improvements have been made to keep various types of administrators informed about their status within the system, thereby enhancing communication and operational flow.
* **Bug Fixes**: This release addresses numurous bugs as part of our quest to provide a quality product.

## Enhancements

### Backend Pagination, Sorting & Searching

The latest update brings a significant enhancement to data navigation within the portal. We've added page-based pagination as an alternative to the existing cursor-based pagination, offering users a more intuitive way to sift through data. Moreover, the portal has transitioned from frontend-enabled pagination, sorting, and search functionalities to now leverage these newly integrated services. This shift centralises the logic for these operations, streamlining user experience while improving overall system performance. The change is aimed at making data interaction more efficient and user-friendly, and it paves the way for future scalability and feature extensions.

### Email Notifications

Recognising the need for better, real-time status updates, we've implemented a series of notification mechanisms. These are designed to keep Organisation Administrators, Tenant Administrators, and Global Administrators on top of of any changes to their roles, whether they've been archived, reinstated, or otherwise modified. The result is a more transparent, responsive system that minimises confusion and expedites decision-making processes. By fine-tuning the notification structure, we aim to provide administrators with the timely information they need to manage their responsibilities more effectively.

## Bug Fixes

This release also includes a comprehensive set of bug fixes aimed at enhancing stability, performance and user experience.

### Portal

* Invited Administrators incorrectly move between 'Current' and 'Pending' statuses.
* Archived Organisations incorrectly displayed alongside empty state text in context switching menu.
* Logout modal appears but fails to log the user out after archiving a Tenant.
* Error message displayed during Organisation creation, despite successful record creation.
* Invited Tenant Administrators are not displayed in the 'Pending' status.
* "Credential not found" message appears after opening a verification request.
* 401 error encountered during ATOM delegation for administrators.
* Organisation Administrators can archive Organisations but are unable to reinstate them.
* 404 error encountered when viewing a credential schema.
* No Organisations available for selection when creating a credential schema, despite existing Organisations in the Tenant.
* Error encountered during the action of creating a credential schema.
* New Organisation Administrators are incorrectly invited as Tenant Administrators.
* Screen redirects to 'Current Administrators' tab after adding a new Administrator.
* Newly invited Organisation Administrators do not appear in the 'Pending Administrators' tab.
* Listed Organisations appear clickable but are not.
* Organisations list fails to appear, with an error message displayed.
* 'Registered' End User invitations incorrectly appear in the 'Pending Invitations' tab.
* Pagination buttons displayed in search results on the Tenants page.
* "Delete" incorrectly labeled as "Archive" for Verification Response.
* Claim 'tid' not found when Organisation Administrators access the 'Administrators' section in the Organisation.
* Clicking "Back to Organisations" yields no action.
* Missing sub-headings for Organisation pages.
* Missing sub-headings for Tenant pages.
* Incorrect label displayed when viewing a Pending Connection.
* Empty list of archived Organisations displayed for Organisation Administrators.
* "View details" option incorrectly displayed for archived Organisations.
* Incorrect information displayed when viewing a verification request.
* Logout modal appears during editing of archived Tenant details.
* Tenant Administrators, who aren't Organisation Administrators, are incorrectly prompted to set up a passphrase.
* No redirection to the 'View Tenant Details' page when clicking on 'Tenant > View'.
* Tooltip box remains displayed and does not disappear.
* Issue credential summary screen underwent UI review.
* "Invalid URI" response returned for Issue credential QR Code in Wallet.
* No redirection to login screen from the "Forgot your password?" screen.
* Array attributes are not rendered correctly when viewing a credential.
* Administrators tables are not sorted by name.
* Error "Organisation already exists" displayed inaccurately.

### Wallet

* Typo corrected in toast message when a Wallet is removed from a Tenant.

### API

We did a review of email notification content and corrected the following bugs:

* Security rights are missing after clicking an invitation link.
* Incorrect email text for Organisation Administrators when a Tenant is deactivated.
* Incorrect email text for Tenant Administrators regarding archived Organisations.
* Incorrect inviter name displayed in email notifications for invited administrators.
* Absence of email notifications for Tenant Administrators upon Tenant reinstatement.
* Multiple identical emails received about being removed as a Global and Organisation Administrator.
* Incorrect error message displayed for exceeding the character limit in invitation messages.
* Revised functionality for resending emails to distinguish between active existing users and newly onboarded inactive users.
* Missing link to Portal in email notifications for reinstating Organisations or Tenants.
* Absence of email instructions during the password reset flow.

Additional bugs that were fixed:

* Resolved issue where page_count and records_count don't update when filters are applied in ATOM.
* Deadlock issue detected and resolved in Vault.
* Added missing message for empty state screen when Organisations are archived in the Portal.
* Corrected missing security rights for new Global Administrators in ATOM.
* Addressed missing documentation for POST /end_users/invitations.
* Fixed link styling issue in Meeco Signature Generator.
* Investigated and addressed errors in IDP service.
* Resolved issues with the invitation flow.
* Enabled viewing of Privacy Policy and Terms & Conditions during the signup process without losing signup state.
* Fixed issue where search by DID doesn't work in the "End Users" page.
* Resolved mismatch in DID document public key type and encoding.


## Deprecations and EOL

None
