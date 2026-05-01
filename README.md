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

## Deploying to Production

**These steps require cluster admin access on the `software-dev` cluster**

Switch your Kubernetes context to the `software-dev` cluster:
```bash
% kubectl config use-context software-dev         
Switched to context "software-dev".
```

Ensure that your token is valid before continuing:
```bash
% kubectl get pods -n keycloak
NAME                                     READY   STATUS      RESTARTS       AGE
kc-themes-explorer                       1/1     Running     0              5s
keycloak-0                               1/1     Running     323            164d
keycloak-postgresql-0                    1/1     Running     4 (106d ago)   164d
move-keycloak-theme-69xdz                0/1     Error       0              164d
move-keycloak-theme-9m92t                0/1     Error       0              164d
move-keycloak-theme-gl8p2                0/1     Error       0              164d
move-keycloak-theme-gsjtk                0/1     Error       0              164d
move-keycloak-theme-t2dnx                0/1     Error       0              164d
rsync-data-keycloak-postgresql-0-g8zlv   0/1     Completed   0              164d
```

NOTE: if you see a 403 error when running `kubectl get pods`, then your token has likely expired.

When this happens: Login to [Rancher](https://gonzo-rancher.ncsa.illinois.edu/dashboard/c/c-m-wcsrbcv4/explorer#cluster-events), choose the `software-dev` cluster, and click "Download KubeConfig" at the top-right corner. Replace your existing token in `~/.kube/config` with the value from the newly-downloaded file. 

Create a `kc-themes-explorer` Pod if it does not already exist (no-op if it exists):
```bash
% kubectl apply -f pvc-explorer.keycloak.software-dev.yaml -n keycloak 
pod/kc-themes-explorer created
```

Once it starts, shell into the container:
```bash
% kubectl exec -it kc-themes-explorer -n keycloak -- bash
root@kc-themes-explorer:/opt#
```

Within the container, run the following commands:
```bash
apt-get update && apt-get install ca-certificates git
git clone https://github.com/healthyregions/sdohplace-keycloak-theme
mv sdohplace-keycloak-theme/theme/sdohplace-discovery/ ../bitnami/keycloak/themes/
```

This will install git, then clone/copy our new theme folder into the list of "themes" that Keycloak can see :tada:
