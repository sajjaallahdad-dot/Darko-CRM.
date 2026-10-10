# Darco Menu

Standalone Persian QR menu for Darco, hosted as a static site on GitHub Pages and connected to the existing Supabase project **Darko os Project**.

## Pages
- `index.html`: customer-facing menu, public read-only access.
- `admin.html`: admin sign-in, category and item management, image upload.

## Database and security
The isolated menu tables are `menu_categories`, `menu_items`, and `menu_admins`. Row Level Security is enabled. Visitors can only read active categories and available products. Authenticated users can manage menu content only when their user ID is listed in `public.menu_admins` with `is_active = true`. Images are stored in the public `menu-images` bucket; uploads and modifications are restricted to menu admins.

## One-time admin setup
1. In Supabase Dashboard → Authentication → Users, create the manager's user.
2. Copy the user's UUID.
3. In SQL Editor, run:
```sql
insert into public.menu_admins (user_id, is_active)
values ('PASTE-AUTH-USER-UUID-HERE', true)
on conflict (user_id) do update set is_active = true;
```
4. Open `admin.html` and sign in with that user's email and password.

## Publish
In GitHub repository Settings → Pages, select **Deploy from a branch**, branch `main`, folder `/(root)`, then save. GitHub Pages publishes static files; the Supabase URL and publishable key in the HTML are client-safe. Never put a Supabase service-role key in browser code.

Note: GitHub Pages access from Iran is not guaranteed without a VPN; test the final URL on the network and phones used by customers.
