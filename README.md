# Lab Seminar V2 - Improvement Plan

## Goal
Improve the category page UX based on `wonder.txt` requirements:
- Show uploaded materials clearly in the category page.
- Provide a delete action next to each `View Session` action.
- Move upload inputs behind a separate side upload button.

## Plan
1. Analyze current UI and behavior in `category.html`.
2. Keep `Uploaded Materials` as the main visible section.
3. Move upload form into a right-side drawer opened by a floating `Upload` button.
4. Ensure each listed session has both `View Session` and `Delete Session` actions.
5. Keep IndexedDB persistence and BroadcastChannel sync for add/delete/reorder updates.
6. Validate no stale JS references remain after refactoring.
7. Record implementation results in `RESULT.md` (Korean analysis).
8. Log user questions and assistant answers with timestamps in a new txt file.
