# Content Model

Concise data model for ideas, voting, roadmap, changelog, comments, and subscriptions.

- UI: embeds-only
- Access: public read-only viewing; all write operations require app SSO session (IdP behind the app)

## Users

- Fields: `id`, `ssoUserId`, `name`, `email`, `role`, `createdAt`, `updatedAt`
- Access: not public; read/write requires authenticated (self or admin)

## Ideas

- Fields: `id`, `title`, `summary`, `details`, `status`, `category`, `tags[]`, `ownerId`, `createdById`, `createdAt`, `updatedAt`, `publishedAt`, `archivedAt`
- Statuses: `draft`, `open`, `under_review`, `planned`, `in_progress`, `completed`, `declined`, `archived`
- Access: read-only for public; create/update/delete requires authenticated (owner or admin)

## Votes

- Fields: `id`, `ideaId`, `voterId`, `weight`, `createdAt`, `updatedAt`, `revokedAt`
- Statuses: `active`, `revoked`
- Access: public read-only aggregates only (no individual votes); cast/revoke requires authenticated

## Roadmap Items

- Fields: `id`, `title`, `summary`, `details`, `status`, `theme`, `ownerId`, `ideaIds[]`, `createdById`, `createdAt`, `updatedAt`, `publishedAt`, `archivedAt`
- Statuses: `draft`, `planned`, `in_progress`, `completed`, `archived`
- Access: read-only for public; create/update/delete requires authenticated (admin/editor)

## Changelog Entries

- Fields: `id`, `title`, `summary`, `details`, `status`, `releaseDate`, `tags[]`, `ownerId`, `roadmapItemIds[]`, `createdById`, `createdAt`, `updatedAt`, `publishedAt`, `archivedAt`
- Statuses: `draft`, `published`, `archived`
- Access: read-only for public; create/update/delete requires authenticated (admin/editor)

## Comments

- Fields: `id`, `body`, `status`, `targetType`, `targetId`, `parentId`, `authorId`, `createdAt`, `updatedAt`, `deletedAt`
- Statuses: `visible`, `hidden`, `deleted`
- Access: read-only for public; create/update/delete requires authenticated (author or moderator)

## Subscriptions/Notifications

- Fields: `id`, `userId`, `targetType`, `targetId`, `channel`, `frequency`, `status`, `lastSentAt`, `createdAt`, `updatedAt`, `unsubscribedAt`
- Statuses: `active`, `paused`, `unsubscribed`
- Access: not public; read/write requires authenticated (owner or admin)
