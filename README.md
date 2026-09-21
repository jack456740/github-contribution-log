# Contribution 1: LibrePhotos Frontend Contribution

**Contribution Number:** 1  
**Student:** Alvin Ray Rogers Jr.  
**Issue:** [LibrePhotos Issue #545 — Add option to only hide image from general photo stream](https://github.com/LibrePhotos/librephotos/issues/545)  
**Status:** Phase III Complete

---

## Why I Chose This Issue

LibrePhotos Issue #545 proposes adding an option to hide an image from the
general photo stream while keeping the image accessible in other areas,
such as albums. This issue interests me because it involves both frontend
behavior and the way the frontend interacts with the LibrePhotos backend to
determine which images are displayed. I chose this issue because it combines
software engineering, frontend development, and understanding API behavior,
which aligns with my computer science background and the goals of this AI301
project.

I am also interested in learning more about how an open-source application
organizes and filters user data across different views. By working on this
issue, I hope to better understand the existing LibrePhotos frontend, its
backend endpoints, and its E2E testing structure while making sure that my
work is independently reviewed and verified.

---

## Understanding the Issue

### Problem Description

LibrePhotos Issue #545 proposes adding an option to only hide an image from
the general photo stream. The issue describes a situation where hiding an
image that has been added to an album can cause the album to no longer appear
by default. The requested behavior is similar to an archive feature, where
an image can be removed from the general photo stream while remaining
available in an album.

### Expected Behavior

LibrePhotos should provide a way for users to hide an image from the general
photo stream without removing the image from albums where it has been
organized. The image should remain accessible through the appropriate album
while no longer appearing in the normal photo stream.

### Current Behavior

When an image is hidden, the current behavior can cause an album containing
that image to disappear by default. This means that the current hide
functionality can affect the visibility of the image beyond the general
photo stream.

### Affected Components

The affected components include the LibrePhotos frontend and the backend
functionality responsible for filtering and returning images. The frontend
uses API endpoints to load photo and album information, while the backend
provides filtering for hidden images. The relevant frontend components,
backend API logic, and existing E2E tests need to be examined to determine
where the current behavior originates.

---

## Reproduction Process

### Environment Setup

I set up the LibrePhotos development environment locally using Docker
Desktop and WSL2 on Windows.

The initial Docker startup encountered an issue with Windows CRLF line
endings in the frontend and backend shell entrypoint scripts. The frontend
container reported:

`/usr/bin/env: 'bash\r': No such file or directory`

and the backend container reported:

`exec /entrypoint.sh: no such file or directory`

I converted the affected shell scripts from CRLF to LF line endings:

- `deploy/docker/frontend/entrypoint.sh`
- `deploy/docker/backend/entrypoint.sh`

After correcting the line endings, the Docker Compose development
environment started successfully.

I then configured a local test-photo directory and added a test image.
LibrePhotos successfully scanned the image and processed it through the
photo-processing jobs.

### Steps to Reproduce

1. Start the LibrePhotos development environment locally using Docker.
2. Add a test image to the configured LibrePhotos scan directory.
3. Wait for LibrePhotos to scan the image and make it available in the
   Photos view.
4. Create a new user album.
5. Add the test image to the new album.
6. Open the album and verify that the test image is visible.
7. Return to the general Photos stream.
8. Select the test image.
9. Open the Photo Actions menu and select **Hide**.
10. Return to the Albums view.
11. Observe that the album containing the hidden image is no longer visible.

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** Local LibrePhotos screenshots showing the test image
  before hiding and the album becoming unavailable after the image was hidden.
- **My findings:** The issue was successfully reproduced locally. Before the
  image was hidden, the test image was visible inside the user-created
  album. After selecting **Hide** for the image, the image was hidden from
  the general Photos stream and the album containing the image was no longer
  visible in the Albums view.

---

## Solution Approach

### Analysis

The current behavior is related to how LibrePhotos represents and filters
hidden photos.

The `Photo` model contains a `hidden` field that represents whether a photo
is hidden. The normal visible-photo behavior excludes photos marked as
hidden.

The album listing logic also considers whether an album has visible photos.
When the only photo in a user-created album is marked as hidden, the album's
visible photo count can become zero. As a result, the album can be excluded
from the normal album listing.

This explains why hiding a photo can cause the album containing that photo to
disappear even though the photo is still associated with the album.

### Proposed Solution

Separate the concept of hiding a photo from the visibility of the user's
album.

The existing **Hide** behavior should continue to prevent the photo from
appearing in the general Photos stream. However, hiding the photo should not
cause the user-created album containing that photo to disappear.

The photo-to-album relationship should remain intact, allowing the photo to
remain accessible through the album.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:**  
The current Hide functionality removes a photo from the general photo stream,
but can also cause a user-created album containing that photo to disappear.
The desired behavior is for hiding a photo to affect the general photo
stream without affecting the existence or accessibility of its album.

**Match:**  
Inspect the existing photo hiding implementation, album listing logic,
serializers, and E2E tests to identify how hidden photos are filtered in
different views. Reuse existing LibrePhotos patterns rather than introducing
a separate representation of hidden photos unless the existing design
requires it.

**Plan:**

1. Inspect the photo hiding implementation and confirm how the `hidden`
   field is updated when a user selects **Hide**.
2. Inspect the user-created album listing logic, particularly how album
   photo counts are calculated and how albums with hidden photos are
   filtered.
3. Determine how album detail views retrieve their associated photos and
   confirm whether hidden photos remain associated with the album.
4. Modify the album visibility/filtering behavior so that a hidden photo does
   not cause an otherwise valid user-created album to disappear.
5. Preserve the existing behavior where hidden photos do not appear in the
   general Photos stream.
6. Add or update automated tests covering a hidden photo inside a
   user-created album.
7. Run the relevant backend/frontend tests and manually verify the behavior
   in the local LibrePhotos development environment.

**Implement:**  
Implementation will be completed during Phase III. The branch and commits
will be linked here after the implementation is completed.

**Review:**  

- Confirm that only files necessary for Issue #545 are modified.
- Check that the change follows existing LibrePhotos coding patterns.
- Verify that existing Hide behavior is not unintentionally changed.
- Verify that user-created albums remain visible when their photos are
  hidden.
- Review the automated tests for adequate coverage.
- Review the final diff before creating a pull request.

**Evaluate:**  

The fix will be verified by reproducing the original scenario:

1. Add a photo.
2. Create an album containing the photo.
3. Confirm the photo appears in the album.
4. Hide the photo.
5. Confirm the photo is removed from the general Photos stream.
6. Confirm the album remains visible.
7. Open the album and confirm the photo remains accessible.
8. Unhide the photo and confirm it returns to the general Photos stream.

## Testing Strategy

### Regression Test

Added a regression test:

`test_hiding_only_photo_does_not_hide_user_album`

The test verifies that:

1. A user-created album is created.
2. A photo is added to the album.
3. The photo is hidden using the existing Hide endpoint.
4. The user's album list is requested.
5. The album remains visible.
6. The hidden photo remains associated with the album.

### Test Results

The existing album media type test file was run after the implementation:

```text
Ran 11 tests
OK
```
### Manual Testing

I reproduced the original issue in the local LibrePhotos development
environment before implementing the fix.

The original behavior was:

1. A photo was added to a user-created album.
2. The photo was visible in the album.
3. The photo was hidden using the Hide action.
4. The album containing the hidden photo was no longer visible.

After the implementation, the regression test confirmed that the album
remains visible when its only photo is hidden.

---

## Implementation Notes

### Phase III Progress

The root cause was identified in the user album listing logic in
`apps/backend/api/views/albums.py`.

The album list calculated its photo count using only photos where
`hidden=False`. When the only photo in an album was hidden, the photo count
became zero and the album was excluded from the user's album list.

The implementation changes the album photo count to include all photos
associated with the user-created album, regardless of whether a photo is
hidden.

### Code Changes

- **`apps/backend/api/views/albums.py`**
  - Updated the user album photo count to include hidden photos.

- **`apps/backend/api/tests/albums/test_album_media_type_filter.py`**
  - Added a regression test for Issue #545.

### Key Commit

**Commit message:** `Fix hidden photos in user album list`

The implementation was committed and pushed to the `dev` branch of my fork.

### Branch Link

[LibrePhotos dev branch](https://github.com/jack456740/librephotos/tree/dev)

---

## Pull Request

**PR Link:** Not submitted yet.

**Status:** Phase III implementation complete; ready for Phase IV review and polish.

---

## Learnings & Reflections

### Technical Skills Gained

I learned how LibrePhotos uses Django querysets and annotations to determine
which user-created albums appear in the album list. I also gained experience
tracing behavior across the photo model, photo hiding endpoint, album model,
and album listing view.

### Challenges Overcome

The main challenge was determining why hiding a photo caused its album to
disappear even though the photo-to-album relationship was not removed.

Inspecting the album listing query showed that the photo count explicitly
excluded hidden photos. This explained why an album containing only hidden
photos was filtered from the album list.

### What I'd Do Differently Next Time

I would inspect the relevant queryset and existing tests earlier in the
process. This would make it easier to identify the root cause and create the
regression test before making implementation changes.

---

## Resources Used

- [LibrePhotos Issue #545](https://github.com/LibrePhotos/librephotos/issues/545)
- [LibrePhotos Repository](https://github.com/LibrePhotos/librephotos)
- LibrePhotos contribution guidelines
- LibrePhotos existing album and photo test suites
