schema.prisma

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

enum ComponentType {
  BUTTON
  FORM
  TABLE
  CHART
  OTHER
}

model User {
  id        String   @id @default(uuid())
  email     String   @unique @notNull
  name      String?
  projects  Project[]
  sessions  Session[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Project {
  id        String    @id @default(uuid())
  name      String    @unique @notNull
  userId    String    @notNull
  user      User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  components Component[]
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt

  @@index([userId])
}

model Component {
  id         String       @id @default(uuid())
  projectId  String       @notNull
  project    Project      @relation(fields: [projectId], references: [id], onDelete: Cascade)
  name       String       @notNull
  type       ComponentType @default(OTHER)
  templateId String?
  template   Template?    @relation(fields: [templateId], references: [id])
  data       Json?
  createdAt  DateTime     @default(now())
  updatedAt  DateTime     @updatedAt

  @@index([projectId])
  @@index([templateId])
}


model Template {
  id        String     @id @default(uuid())
  name      String     @unique @notNull
  type      ComponentType @default(OTHER)
  data      Json?
  components Component[]
  createdAt DateTime    @default(now())
  updatedAt DateTime    @updatedAt
}


model Session {
  id        String   @id @default(uuid())
  userId    String   @notNull
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  token     String   @unique @notNull
  expiresAt DateTime @notNull
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([userId])
}
```