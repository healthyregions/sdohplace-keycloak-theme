# SDOH Place Keycloak Theme

This package provides a login theme named `sdohplace-discovery` for Keycloak. It keeps the default Keycloak login layout and only adjusts branding:

- `Nunito` / `Fredoka` font pairing
- SDOH Place color palette
- decorative background accents
- project logo above the login header

## Install

Copy the `theme/sdohplace-discovery` directory into the Keycloak server `themes/` directory.

If you prefer an archive deployment, package this folder structure into a JAR with:

- `META-INF/keycloak-themes.json`
- `theme/sdohplace-discovery/login/...`

Then place that JAR in the Keycloak `providers/` directory.

## Activate

1. Open the Keycloak Admin Console.
2. Select your realm.
3. Open `Realm settings`.
4. Open the `Themes` tab.
5. Set `Login theme` to `sdohplace-discovery`.
6. Save.

## Local Preview

Run a local Keycloak preview server from this folder:

```bash
docker compose up
```

Then open:

- `http://localhost:8080`
- Admin login: `admin` / `admin`

After Keycloak starts:

1. Open the Admin Console.
2. Create or select a test realm.
3. Open `Realm settings` → `Themes`.
4. Set `Login theme` to `sdohplace-discovery`.
5. Save.
6. Open the realm login page and refresh while editing `theme/sdohplace-discovery/login/resources/css/styles.css`.

## Refresh During Testing

If theme changes do not appear immediately, restart Keycloak with theme caching disabled for development, or clear the server cache directory `data/tmp/kc-gzip-cache` before restarting.