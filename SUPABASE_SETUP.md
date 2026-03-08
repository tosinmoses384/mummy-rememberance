# SUPABASE SETUP – So ticks are shared with all your brothers and sisters

When you complete this once, everyone who opens the plan link will see the same checklist. If Isaac ticks something, Daddy Ruth will see it when he opens the link (and the other way round).

## Step 1: Create a free Supabase account and project

1. Go to https://supabase.com and sign up (free).
2. Click "New project". Choose a name (e.g. mummy-remembrance), a password for the database (save it somewhere safe), and a region. Click Create.
3. Wait until the project is ready (green check).

## Step 2: Create the table

1. In the left menu click **SQL Editor**.
2. Click **New query**.
3. Paste this exactly and click **Run**:

```sql
create table if not exists remembrance_plan (
  id text primary key,
  checked boolean not null default false
);

alter table remembrance_plan enable row level security;

create policy "Allow anyone to read"
  on remembrance_plan for select using (true);

create policy "Allow anyone to insert"
  on remembrance_plan for insert with check (true);

create policy "Allow anyone to update"
  on remembrance_plan for update using (true);
```

4. You should see "Success. No rows returned."

## Step 3: Get your URL and key

**Project URL** and **API key** are in different places:

1. **Project URL**
   - In the left menu click **Project Settings** (gear icon).
   - Click **General** (the first item under PROJECT SETTINGS).
   - Copy the **Project ID** (the short string, e.g. `woaigrdbqvlxwbvqjjeg`).
   - The Project URL is: **`https://` + your Project ID + `.supabase.co`**  
     Example: if Project ID is `woaigrdbqvlxwbvqjjeg`, then Project URL is `https://woaigrdbqvlxwbvqjjeg.supabase.co`.

2. **API key (anon key)**
   - Still under Project Settings, click **API Keys** in the left menu.
   - If you see two tabs — **"Publishable and secret API keys"** and **"Legacy anon, service_role API keys"** — open the **"Legacy anon, service_role API keys"** tab.
   - Under **"anon public"**, click **Copy** to copy the long key (it starts with `eyJ...`). Use this as your anon key.

## Step 4: Put them in the plan page

1. Open `MUMMY_1_YEAR_REMEMBRANCE_PLAN.html` in a text editor (or Cursor).
2. Find these two lines near the top of the script (around line 220):

```javascript
var SUPABASE_URL = '';  // e.g. 'https://xxxxx.supabase.co'
var SUPABASE_ANON_KEY = '';  // long string from Project Settings > API
```

3. Put your Project URL between the first quotes, and your anon key between the second quotes. Example:

```javascript
var SUPABASE_URL = 'https://abcdefgh.supabase.co';
var SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

4. Save the file.

## Step 5: Redeploy to Netlify

1. Copy the updated `MUMMY_1_YEAR_REMEMBRANCE_PLAN.html` into your `netlify-deploy` folder (replace the existing `index.html` there, or copy the whole `<script>` block and the `SUPABASE_URL` / `SUPABASE_ANON_KEY` values into `netlify-deploy/index.html`).
2. Drag the `netlify-deploy` folder again to https://app.netlify.com/drop (or deploy however you did the first time).

---

**Done.** When you and your siblings open the Netlify link, you will all see the same ticks. The anon key is safe to have in the page: it only allows read/write to this one table and is meant for public use.

If you leave `SUPABASE_URL` and `SUPABASE_ANON_KEY` empty, the page still works: ticks are saved only on each device (like before).
