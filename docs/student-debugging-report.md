# LaunchDesk Debugging Report

Project: LaunchDesk Debugging Lab  
Student or group name: Savi  
Date: 2026-05-12

## Bug 01 - Submit handler typo

### Problem

Submitting the add-check form did nothing.

### Steps to Reproduce

1. Open the app.
2. Fill in a title and category.
3. Click Add check.

### Evidence

Console error indicating the submit handler function was not defined.

### Cause

The event listener called a misspelled function name.

### Fix Applied

Updated the submit handler to call `handleAddCheck` in [js/app.js](js/app.js).

### Test Result

After the change, submitting adds a new row and clears the form.

### Commit Message

fix: wire submit handler

### Learning

Small typos in event handlers can completely block UI flows.

## Bug 02 - Required field validation

### Problem

The form allowed adding a check when only one required field was filled.

### Steps to Reproduce

1. Enter a title only.
2. Leave category empty.
3. Click Add check.

### Evidence

An item was added despite missing a required field.

### Cause

Validation should fail when either field is missing.

### Fix Applied

Verified the condition uses `!title || !category` in [js/app.js](js/app.js).

### Test Result

Submitting with only one required field now shows the error message.

### Commit Message

n/a (already correct on main)

### Learning

Validation logic is easy to invert when reading boolean expressions quickly.

## Bug 03 - Storage key mismatch

### Problem

Saved checks disappeared after refresh.

### Steps to Reproduce

1. Add a new check.
2. Refresh the page.
3. Check the list.

### Evidence

Local storage showed a key that did not match the load key.

### Cause

Load and save keys were different.

### Fix Applied

Aligned the load key with the save key in [js/app.js](js/app.js).

### Test Result

Refresh now keeps saved checks.

### Commit Message

fix: align storage key

### Learning

Persistence bugs are often just mismatched keys.

## Bug 04 - Search scope

### Problem

Searching for category or status terms did not return results.

### Steps to Reproduce

1. Type "SEO" in search.
2. Observe results.
3. Clear search.

### Evidence

Only owner matches were returned.

### Cause

Filter only checked the owner field.

### Fix Applied

Expanded search to title, category, priority, status, and owner in [js/app.js](js/app.js).

### Test Result

Searching by category/status now returns expected rows.

### Commit Message

fix: broaden checklist search

### Learning

Search should match user expectations, not just a single field.

## Bug 05 - Status filter mismatch

### Problem

Status filter showed the wrong rows.

### Steps to Reproduce

1. Select status filter "Fixed".
2. Review the checklist.
3. Compare results with actual fixed items.

### Evidence

Rows were filtered by priority instead of status.

### Cause

The filter compared `check.priority` to the selected status.

### Fix Applied

Switched the comparison to `check.status` in [js/app.js](js/app.js).

### Test Result

Status filter now matches the selected status.

### Commit Message

fix: filter by status

### Learning

Double-check property names when filtering derived views.

## Bug 06 - Status badge slug

### Problem

"In Progress" status badge had no styling.

### Steps to Reproduce

1. Find a row with status "In Progress".
2. Inspect the badge class.
3. Compare to CSS selectors.

### Evidence

Class was `status-in progress` (space) but CSS expected `status-in-progress`.

### Cause

Status string was not slugged.

### Fix Applied

Replaced spaces with dashes when building the status class in [js/app.js](js/app.js).

### Test Result

Badge now uses the correct class and styling.

### Commit Message

fix: slug status badge class

### Learning

UI styling bugs often come from class name mismatches.

## Bug 07 - Fixed count metric

### Problem

Fixed count and readiness score were wrong.

### Steps to Reproduce

1. Look at fixed count.
2. Compare against rows marked Fixed.
3. Change a status to Fixed and watch metrics.

### Evidence

Code checked for "Complete" instead of "Fixed".

### Cause

Wrong status label in the metric calculation.

### Fix Applied

Updated fixed count to use "Fixed" in [js/app.js](js/app.js).

### Test Result

Fixed count and score updated correctly.

### Commit Message

fix: count fixed checks

### Learning

Derived metrics are fragile when labels drift.

## Bug 08 - Due soon metric

### Problem

Due this week was counting items due far in the future.

### Steps to Reproduce

1. Compare due dates vs. Due this week count.
2. Add a new item due today.
3. Watch the counter.

### Evidence

Logic counted dates greater than 7 days away.

### Cause

Comparison was reversed.

### Fix Applied

Counted items where days are between 0 and 7 in [js/app.js](js/app.js).

### Test Result

Due this week now reflects near-term items only.

### Commit Message

fix: count checks due soon

### Learning

Date logic needs explicit bounds to avoid off-by-one errors.

## Bug 09 - Delete button

### Problem

Delete buttons did nothing.

### Steps to Reproduce

1. Click the delete (x) button on a row.
2. Observe whether the row is removed.
3. Check the activity log.

### Evidence

Listener looked for `data-delete-id` while the DOM used `data-remove-id`.

### Cause

Mismatch between dataset key and selector.

### Fix Applied

Aligned selector and dataset access to `data-remove-id` in [js/app.js](js/app.js).

### Test Result

Row is removed and log updates.

### Commit Message

fix: wire delete button

### Learning

Event delegation breaks easily when data attributes drift.

## Bug 10 - Status change persistence

### Problem

Changing a status updated the row but not metrics or storage.

### Steps to Reproduce

1. Change a row status to Fixed.
2. Check metrics.
3. Refresh the page.

### Evidence

Metrics did not change and refresh lost the update.

### Cause

Status updates were not saved or re-filtered.

### Fix Applied

Saved changes and re-applied filters after status change in [js/app.js](js/app.js).

### Test Result

Metrics update and status persists after refresh.

### Commit Message

fix: persist status changes

### Learning

State updates should always refresh derived views and storage.

## Bug 11 - Reset demo data

### Problem

Reset demo failed with a network error.

### Steps to Reproduce

1. Click Reset demo.
2. Open Network tab.
3. Observe the request.

### Evidence

Request went to a non-existent JSON file.

### Cause

Incorrect file path in the fetch call.

### Fix Applied

Updated fetch path to `data/launch-checks.json` in [js/app.js](js/app.js).

### Test Result

Reset reloads demo data successfully.

### Commit Message

fix: load demo checks json

### Learning

Network 404s are quick clues for broken file paths.

## Bug 12 - CSV export title

### Problem

Exported CSV had blank titles.

### Steps to Reproduce

1. Click Export CSV.
2. Open the downloaded file.
3. Check the Title column.

### Evidence

Title column was empty in the export.

### Cause

Export used `check.name` instead of `check.title`.

### Fix Applied

Switched the export field to `check.title` in [js/app.js](js/app.js).

### Test Result

Exported file includes check titles.

### Commit Message

fix: export check titles

### Learning

Always validate output files, not just the UI.
