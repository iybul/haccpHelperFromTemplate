<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://user-images.githubusercontent.com/9113740/201498864-2a900c64-d88f-4ed4-b5cf-770bcb57e1f5.png">
  <source media="(prefers-color-scheme: light)" srcset="https://user-images.githubusercontent.com/9113740/201498152-b171abb8-9225-487a-821c-6ff49ee48579.png">
  <img alt="Shows all of the tools in the stack for this template, also listed in the README file." src="https://user-images.githubusercontent.com/9113740/201498152-b171abb8-9225-487a-821c-6ff49ee48579.png">
</picture>

<div align="center"><strong>Next.js 13 Admin Dashboard Template</strong></div>
<div align="center">Built with the Next.js App Router</div>
<br />
<div align="center">
<a href="http://admin-dash-template.vercel.sh/">Demo</a>
<span> · </span>
<a href="https://vercel.com/templates/next.js/admin-dashboard-tailwind-planetscale-react-nextjs">Clone & Deploy</a>
<span>
</div>

## Overview

This is a starter template using the following stack:

- Framework - [Next.js 13](https://nextjs.org/13)
- Language - [TypeScript](https://www.typescriptlang.org)
- Auth - [NextAuth.js](https://next-auth.js.org)
- OLD METHOD: Database - [PlanetScale](https://planetscale.com)
- NEW METHOD: Database - [Supabase](https://supabase.com)
- Deployment - [Vercel](https://vercel.com/docs/concepts/next.js/overview)
- Styling - [Tailwind CSS](https://tailwindcss.com)
- Components - [Tremor](https://www.tremor.so)
- Analytics - [Vercel Analytics](https://vercel.com/analytics)
- Linting - [ESLint](https://eslint.org)
- Formatting - [Prettier](https://prettier.io)

This template uses the new Next.js App Router. This includes support for enhanced layouts, colocation of components, tests, and styles, component-level data fetching, and more.

## Getting Started

THIS IS THE ORIGINAL METHOD: After creating an account with PlanetScale, you'll need to create a new database and retrieve the `DATABASE_URL`. Optionally, you can use Vercel integration, which will add the `DATABASE_URL` to the environment variables for your project.

THIS IS MY METHOD: After creating an account with Supabase: Create a new organization, copy your URL anon and service role keys from the API settings page, get your connectionstring from the Project Settings/Database section, Supabase URL and ANON key get added to your `.env.local` local file


```
# https://vercel.com/integrations/planetscale
DATABASE_URL=

# https://supabase.com/
NEXT_PUBLIC_SUPABASE_URL=YOUR_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY

NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET= # Linux: `openssl rand -hex 32` or go to https://generate-secret.now.sh/32

# https://next-auth.js.org/providers/github
GITHUB_ID=
GITHUB_SECRET=
```
NEW METHOD: Next, inside Supabase, create a SQL script to build a table for employees.

```
CREATE TABLE employeeWriter (
 id bigint generated by default as identity primary key,
 user_id uuid references auth.users not null,
 user_email text,
 title text,
 loads text,
 reps text,
 inserted_at timestamp with time zone default timezone('utc'::text, now()) not null
);

alter table employeeWriter enable row level security;

create policy "Employees can create tables." on employeeWriter for
   insert with check (auth.uid() = user_id);

create policy "Employees can update company own tables." on employeeWriter for
   update using (auth.uid() = user_id);

create policy "Employees can not delete company tables." on employeeWriter for
   delete using (auth.uid() = user_id);

create policy "Tables are public." on employeeWriter for
   select using (true);
```

For handling AUTH we use Supabases auth functionality we use

```
npm install @supabase/auth-helpers-nextjs @supabase/supabase-js

npm install @supabase/auth-ui-react @supabase/auth-ui-shared


```


OLD METHOD: Next, inside PlanetScale, create a users table based on the schema defined in this repository.

```
CREATE TABLE `users` (
  `id` int NOT NULL AUTO_INCREMENT,
  `email` varchar(255) NOT NULL,
  `name` varchar(255),
  `username` varchar(255),
  PRIMARY KEY (`id`)
);
```






Insert a row for testing:

```
INSERT INTO `users` (`id`, `email`, `name`, `username`) VALUES (1, 'me@site.com', 'Me', 'username');
```

Finally, run the following commands to start the development server:

```
pnpm install
pnpm dev
```

You should now be able to access the application at http://localhost:3000.
