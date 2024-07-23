## 1. Typeorm

`RoleEntity.ts`

```javascript
import { Entity, Column, PrimaryGeneratedColumn, OneToMany } from 'typeorm';
import { User } from '@entities/UserEntity';

@Entity()
export class Role {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    name: string;

    @OneToMany(() => User, user => user.role)
    users: User[];
}
```

`UserEntity.ts`

```javascript
import { Entity, Column, PrimaryGeneratedColumn, ManyToOne, JoinColumn } from 'typeorm';
import { Role } from '@entities/RoleEntity';

@Entity()
export class User {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    first_name: string;

    @Column()
    last_name: string;

    @Column()
    hashed_password: string;

    @ManyToOne(() => Role, role => role.users)
    @JoinColumn({ name: 'role_id' })
    role: Role;
}
```

## 2. Prisma

```prisma
datasource db {
  provider = "postgresql"
  url      = "postgresql://postgres:root@localhost:5432/env_store"
}

generator client {
  provider = "prisma-client-js"
}

model Role {
  id    Int     @id @default(autoincrement())
  name  String
  users User[]
}

model User {
  id             Int     @id @default(autoincrement())
  first_name     String
  last_name      String
  hashed_password String
  role_id        Int
  role           Role    @relation(fields: [role_id], references: [id])
}
```
