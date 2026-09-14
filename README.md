# Contribution 1: LibrePhotos Frontend Contribution

**Contribution Number:** 1
**Student:** Alvin Ray Rogers Jr.
**Issue:** [LibrePhotos Issue #545 — Add option to only hide image from general photo stream](https://github.com/LibrePhotos/librephotos/issues/545)
**Status:** Phase I Complete

---

## Why I Chose This Issue

[1-2 paragraphs explaining why this issue interests you, how it matches your skills/learning goals, what you hope to learn]

LibrePhotos Issue #545 proposes adding an option to hide an image from the general photo stream while keeping the image accessible in other areas, such as albums. This issue interests me because it involves both frontend behavior and the way the frontend interacts with the LibrePhotos backend to determine which images are displayed. I chose this issue because it combines software engineering, frontend development, and understanding API behavior, which aligns with my computer science background and the goals of this AI301 project.

I am also interested in learning more about how an open-source application organizes and filters user data across different views. By working on this issue, I hope to better understand the existing LibrePhotos frontend, its backend endpoints, and its E2E testing structure while making sure that my work is independently reviewed and verified.

---

## Understanding the Issue

### Problem Description

LibrePhotos Issue #545 proposes adding an option to only hide an image from the general photo stream. The issue describes a situation where hiding an image that has been added to an album can cause the album to no longer appear by default. The requested behavior is similar to an archive feature, where an image can be removed from the general photo stream while remaining available in an album.

### Expected Behavior

LibrePhotos should provide a way for users to hide an image from the general photo stream without removing the image from albums where it has been organized. The image should remain accessible through the appropriate album while no longer appearing in the normal photo stream.

### Current Behavior

When an image is hidden, the current behavior can cause an album containing that image to disappear by default. This means that the current hide functionality can affect the visibility of the image beyond the general photo stream.

### Affected Components

The affected components include the LibrePhotos frontend and the backend functionality responsible for filtering and returning images. The frontend uses the `/api/albums/date/list/` and `/api/albums/date/<id>` endpoints to load images for the general photo stream, while the backend provides filtering for hidden images. The relevant frontend components, backend API logic, and existing E2E tests will need to be examined to determine where the current behavior originates.


---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
