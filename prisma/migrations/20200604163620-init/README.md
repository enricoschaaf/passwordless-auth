# Migration `20200604163620-init`

This migration has been generated by enricoschaaf at 6/4/2020, 4:36:20 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "public"."users" (
"email" text  NOT NULL ,
    PRIMARY KEY ("email"))

CREATE TABLE "public"."tokens" (
"access" text  NOT NULL ,"confirm" text  NOT NULL ,"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"id" text  NOT NULL ,"userEmail" text  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE UNIQUE INDEX "tokens.confirm" ON "public"."tokens"("confirm")

ALTER TABLE "public"."tokens" ADD FOREIGN KEY ("userEmail")REFERENCES "public"."users"("email") ON DELETE CASCADE  ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20200604163620-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,24 @@
+datasource db {
+  provider = "postgresql"
+  url      = env("DATABASE_URL")
+}
+
+generator client {
+  provider = "prisma-client-js"
+}
+
+model User {
+  email  String  @id
+  tokens Token[]
+  @@map(name: "users")
+}
+
+model Token {
+  id        String   @id
+  access    String
+  confirm   String   @unique
+  createdAt DateTime @default(now())
+  User      User     @relation(fields: [userEmail], references: [email])
+  userEmail String
+  @@map(name: "tokens")
+}
```

