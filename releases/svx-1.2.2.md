# SVX 1.2.2 Release Notes

**Software Release Date**: Sept 20, 2023

**Summary**:

The following is a summary of the key features and enhancements:

* **Frontend Pagination, Sorting and Searching**: Added the ability to sort and paginate items in Verification Templates and Requests.
* **Backend Pagination, Sorting and Searching**: A new feature allows users to filter connections by date connection created.
* **Email Notifications**: Added email notifications for administrators in the event their user status changes, and in the event a Tenant or Organisation that they belong to is archived/reinstated. 
* **Enhancements**: Updated links in page sub-headings so that all links refer to latest sources.
* **Bug Fixes**: This release addresses numurous bugs as part of our quest to provide a quality product. Bugs in this release primarily related to user invitation flows.

## Enhancements

### Frontend Pagination, Sorting & Searching

We have implemented table sorting (A-Z, and Z-A by name) within the Verification Templates and Verification Requests pages.

### Backend Pagination, Sorting & Searching

In this version we have added page-based pagination for connections (sorted by *created_at* descending). 

### Email Notifications

Significant improvements have been made in defining the hierarchical structure of administrators and associated use cases for administrators of different levels and contexts. This enhancement allowed us to minimise the number of notification emails sent to one administartor when they hold multiple administrator roles. 
We also added email notifications for Global Administartors in the event a change occurs related to the status of a Tenant. 

## Bug Fixes

This release also includes a comprehensive set of bug fixes aimed at improving the user experience.

### Portal

* Fixed issue with restoring connection between Wallet and Organisation Provider after archiving and reinstating a Tenant.
* Fixed 500 error shown whilst attempting to view a Verification Template assocaited with an archived Credential Schema.
* Fixed sorting of Credential Schemas.
* Fixed email notification for an Organisation Administrator when a Tenant has been archived / deactivated.
* Fixed navigation between tabs after "Cancel and remove invitation" action for Pending Administartors.
* Fixed the ability to set up an Organisation's passphrase right after adding a new Organisation.
* Fixed 404 error when accessing a Connection after issuing a credential.
* Fixed unnecessary error when Organisation's name is changed.
* Fixed the request to enter a temporary password after clicking on "Navigate to Portal login" in an invitation email, when the provided temporary password has expired.
* Fixed non-working link "click here" during the creation of a new Tenant Application.
* Fixed the tooltip message after establishing a Connection with an existing connected Organisation.
* Fixed issue of displaying a removed Administrator as "Undifined" in the navigation menu.

### API

* The `/version` endpoint now returns the versions of all underlying components as well as the overall SVX version.

## Deprecations and EOL

None
