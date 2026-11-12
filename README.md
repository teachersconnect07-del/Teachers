# Teachers Connect

The original Teachers Connect HTML design is preserved. The content editor is protected by Supabase Auth; publishing writes the shared site content to Supabase, and uploaded images are stored in Cloudinary. Public pages use the published content and update live when Supabase Realtime is enabled by `supabase/schema.sql`.

## Create the required accounts

1. Create a Supabase project at [supabase.com](https://supabase.com/).
2. In Supabase **Project Settings → API**, copy the Project URL and the **publishable** key (or legacy anon key). These are public client values; never use the `service_role` key in this website.
3. In the Supabase SQL Editor, run `supabase/schema.sql`.
4. In Supabase **Authentication → Users**, create the admin user with an email and password. Copy that user's UUID and run:

   ```sql
   insert into public.site_admins (user_id)
   values ('PASTE-THE-ADMIN-USER-UUID-HERE');
   ```

5. Create a Cloudinary account at [cloudinary.com](https://cloudinary.com/). Copy its cloud name from the dashboard. In **Settings → Upload → Upload presets**, create an **unsigned** preset that allows images only and set a reasonable file-size limit (for example 10 MB). Copy the preset name. The browser only needs the cloud name and unsigned preset; do not put a Cloudinary API secret in this app.

## Run locally

1. Copy `.env.example` to `.env.local` and fill in:

   ```dotenv
   VITE_SUPABASE_URL=https://YOUR-PROJECT.supabase.co
   VITE_SUPABASE_ANON_KEY=YOUR-PUBLISHABLE-OR-ANON-KEY
   VITE_CLOUDINARY_CLOUD_NAME=YOUR-CLOUD-NAME
   VITE_CLOUDINARY_UPLOAD_PRESET=YOUR-UNSIGNED-PRESET
   ```

2. Run `npm install`, then `npm run dev`.
3. Open `http://localhost:5173/admin` and sign in with the admin account. The admin page is intentionally not linked from public navigation. Save editor changes, then select **Publish changes**; the public site receives them immediately.

Without a Supabase row yet, the public site starts with the content bundled in the original HTML. The first admin publish creates the shared row. Admin edits are not public until published.

## Arrange page content

In the admin editor, use **Free canvas layout** to choose Home, Conclave, Initiatives, Updates, or the Article template. Drag a dashed-outline image, text element, card, or section to reposition it; select an element and use the slider to resize it. Layouts are proportional to screen width, including on phones. Select **Save layout draft**, then **Publish changes** to apply it to the public site. Use **Reset this page layout** to clear that page's positioning and sizing.

## Deploy to Vercel

Import this folder into Vercel. Use `npm run build` and `dist` (also set in `vercel.json`). Add the four `VITE_...` values above under **Project → Settings → Environment Variables**, then redeploy after changing them.

In Supabase **Authentication → URL Configuration**, set the Site URL to the deployed Vercel URL and add the local development URL and any Vercel preview URLs you use to the Redirect URLs list.

## Image handling

The existing built-in images remain available. Admins can also upload a JPG, PNG, WebP, or GIF through the editor; Cloudinary returns a secure image URL that is saved with the content. The unsigned preset is public by design, so keep its upload restrictions enabled.
