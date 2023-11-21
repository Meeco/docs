# SVX 1.2.5 Release Notes

**Software Release Date**: Nov 22, 2023

**Summary**:

The following is a summary of the key features and enhancements:

* **Enhancements**: Added pagination for end user pages and improved behavior during search: the number and numbering of pages corresponds to the specified search criterion, and also adjusted the display of data when navigating through pages and tabs
* **Loading screen feature**: In cases where an administrator has been archived and no longer has access to the SVX Portal during their session - a loading screen appears while the changes are being applied.
* **Bug Fixes**: Various bug fixes for the portal, including fixes for spelling errors, pagination and search issues, and improvements to the error message displayed when an incorrect email address is entered when inviting an admin.


## Enhancements

### Portal

* Added a pagination feature for End Users pages.
* Implemented a new loading screen.
* Updated notification email footer text and logo.
* Updated text size and wording in Password Reset notification emails.
* Added the option "Delete" for the verification response.



## Bug Fixes

This release also includes bug fixes aimed at improving the user experience.

### Portal

* Fixed display of search results and pagination during the search process or its reset.
* Fixed the issue with "Bad request exception" during the invitation of a new admin to the Organisation.
* Fixed pagination issue in Credential Templates, Verification Templates and Verification requests pages: pagination should be removed when less than 10 results during some removing records process.
* Fixed spelling issue: The word "credential" is misspelt in the description text (sub-heading).
* The tooltip link now opens in a new browser tab.
* Fixed issue with changing the search field icon to a cross when text has been entered.
  

## Various

* Our own custom implementation of the controllers/actions for loading/registering vault keys and organisation DID have been replaced with calls to @meeco/sdk so that now the same format is used in all our services.

## Deprecations and EOL

None
