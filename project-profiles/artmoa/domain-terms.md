# ArtMoa Domain Terms

This document keeps ArtMoa terms consistent across backend, frontend, docs, and
AI-assisted work.

## Core Terms

| Korean | English | Meaning |
| --- | --- | --- |
| 사용자 | User | A service member account. Owned by Identity. |
| 작가 | Artist | A user with artist profile or artist registration flow. |
| 전시회 | Exhibition | A public or pending exhibition entry. |
| 전시장 / 장소 | Venue | A reusable physical place with address and coordinates. |
| 작품 | Artwork | Artwork metadata and images. |
| 자유게시판 | Free Board | General community board. |
| 건의게시판 | Suggestion Board | Suggestions or inquiries from users. |
| 댓글 | Comment | Comment on board posts. |
| 첨부파일 | Attachment | Uploaded file metadata and storage key. |
| 지도 | Map | Map view and venue marker experience. |

## Context Ownership

| Context | Owns |
| --- | --- |
| Identity | Users, OAuth accounts, roles, account status, tokens |
| Board | Free posts, suggestion posts, comments |
| Attachment | Upload metadata, storage keys, file owner linking |
| Venue | Venues, addresses, coordinates, provider place IDs |
| Exhibition | Exhibition lifecycle, approval and publication |
| Artwork | Artwork metadata and media relationships |

## Venue And Exhibition Language

- Venue is not only a raw address. It is the reusable place record.
- Exhibition should link to Venue by ID or a relationship table, depending on
  final exhibition policy.
- Exhibition should not directly own or mutate Venue coordinates.
- Address text and in-building location labels should be named carefully in UI.
  Example: a department store address and "1F gallery" are different concepts.

## Attachment And Rich Content Language

- Store durable attachment references in content as attachment IDs when possible.
- Do not treat temporary object URLs as the permanent source of truth.
- The backend may return a public or signed URL for rendering, but storage key
  and attachment metadata should remain the durable record.
