# SVX 1.2.2 Release Notes

**Software Release Date**: Sept 20, 2023

**Summary**:

The following is a summary of the key features and enhancements:

* **Frontend Pagination, Sorting and Searching**: Added the ability to sort and paginate items in the Verification Templates and Requests.
* **Backend Pagination, Sorting and Searching**: A new feature allows to filter connections by created connection date.
* **Email Notifications**: Expanded use cases for the various types of administrators about their status within the system and notification about archiving/reinstating Tenant or Organisation that administrators are belonging to. 
* **Enhancements**: Updated links in the sub-heading so that all information refers to actual sources.
* **Bug Fixes**: This release addresses numurous bugs as part of our quest to provide a quality product, mainly related to invitation flows.

## Enhancements

### Frontend Pagination, Sorting & Searching

We've implemented support on UI the ability to sort from A-Z, and Z-A by name for tables on Verification Templates and Verification Requests pages.

### Backend Pagination, Sorting & Searching

In this version we've added page-based pagination for connections sorted by *created_at* desc. 

### Email Notifications

Significant improvements have been made in defining the hierarchical structure of administrators and associated use cases for administrators of different levels and contexts. This enhancement allowed to solve issue with the sending multiple emails to one administartor if he holds multiple admin roles 
Added notifications for global administartors is something happens with some Tenant. 

## Bug Fixes

This release also includes a comprehensive set of bug fixes aimed at user experience.

### Portal

* Fixed issue with restoring connection between Wallet and Organisation Provider after archiving and reinstating Tenant
* Fixed 500 error during attempt to open the page of the needed verification template used archived credential schema
* Fixed sorting for Credential Schema
* Fixed email for Organisation Administrator when Tenant has been deactivated
* Fixed navigation between tabs after "Cancel and remove invitation" action for Pending Administartors
* Fixed the ability to set up the passphrase right after adding a new organisation
* Fixed 404 error when accessing connection after issuing a credential
* Fixed unnecessary error when Organisation's name is changed
* Fixed the request to enter a temporary password after clicking on "Navigate to Portal login", when the password has expired
* Fixed non-working link "click "Here" for Tenant Application during attempt to create a new application
* Fixed the tooltip message after establishing connection with already connected organisation
* Fixed issue with the displaying as "Undifined" the archived admin without any access to any context

### API

* Changed association Openapi version to the version of the platform,instead of the gateway component



## Deprecations and EOL

None
