# matrix-authentication-service

![Version: 0.8.2](https://img.shields.io/badge/Version-0.8.2-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: sha-206d45b](https://img.shields.io/badge/AppVersion-sha--206d45b-informational?style=flat-square)

A Helm chart for deploying the matrix authentication service on Kubernetes

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| jessebot |  | <https://github.com/jessebot> |

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| oci://registry-1.docker.io/bitnamicharts | postgresql | 15.5.4 |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| autoscaling.enabled | bool | `false` |  |
| autoscaling.maxReplicas | int | `100` |  |
| autoscaling.minReplicas | int | `1` |  |
| autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| configVolume.existingClaim | string | `""` | name of an existing persistent volume claim to use for matrix-authentication-service config. If provided, ignores mas parameter map |
| configVolume.storage | string | `"500Mi"` | storage capacity for creating a persistent volume |
| configVolume.storageClassName | string | `"default"` | name of storage class for the persistent volume |
| existingMasConfigSecret | string | `""` | Existing Kubernetes Secret for entire matrix authentication service `config.yaml` file. If set, everything under the mas section of the values.yaml is ignored. |
| externalDatabase.database | string | `"mas"` | name of the database to try and connect to |
| externalDatabase.enabled | bool | `false` | enable using an external database *instead of* the Bitnami PostgreSQL sub-chart if externalDatabase.enabled is set to true, postgresql.enabled must be set to false |
| externalDatabase.existingSecret | string | `""` | Name of existing secret to use for PostgreSQL credentials |
| externalDatabase.hostname | string | `""` | hostname of db server. Can be left blank if using postgres subchart |
| externalDatabase.password | string | `"changeme"` | password of matrix-authentication-service postgres user - ignored using exsitingSecret |
| externalDatabase.port | string | `"5432"` | which port to use to connect to your database server |
| externalDatabase.secretKeys.adminPasswordKey | string | `"postgresPassword"` | key in existingSecret with the admin postgresql password |
| externalDatabase.secretKeys.database | string | `"database"` | key in existingSecret with name of the database |
| externalDatabase.secretKeys.databaseHostname | string | `"hostname"` | key in existingSecret with hostname of the database |
| externalDatabase.secretKeys.databaseUsername | string | `"username"` | key in existingSecret with username for matrix to connect to db |
| externalDatabase.secretKeys.userPasswordKey | string | `"password"` | key in existingSecret with password for matrix to connect to db |
| externalDatabase.sslcert | string | `""` | optional: tls/ssl cert for postgresql connections |
| externalDatabase.sslkey | string | `""` | optional: tls/ssl key for postgresql connections |
| externalDatabase.sslmode | string | `""` | sslmode to use, example: verify-full NOTE: SSL not yet supported due to https://github.com/matrix-org/matrix-authentication-service/issues/2799 |
| externalDatabase.sslrootcert | string | `""` | optional: tls/ssl root cert for postgresql connections |
| externalDatabase.username | string | `"mas"` | username of matrix-authentication-service postgres user |
| extraVolumeMounts | list | `[]` |  |
| extravolumes | list | `[]` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` | image pull policy. if image.tag is set to "latest", set to "Always" |
| image.repository | string | `"ghcr.io/matrix-org/matrix-authentication-service"` |  |
| image.tag | string | `""` | Overrides the image tag whose default is the chart appVersion. |
| imagePullSecrets | list | `[]` |  |
| ingress.annotations | object | `{}` | annotations for ingress resource |
| ingress.className | string | `""` | className for ingress. Most people set this to "nginx" |
| ingress.enabled | bool | `false` | enable ingress to reach the matrix authentication service outside the cluster |
| ingress.hosts[0].host | string | `"chart-example.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| ingress.tls | list | `[]` |  |
| livenessProbe.enabled | bool | `false` | enable a liveness probe on the deployment |
| livenessProbe.httpGet.path | string | `"/"` |  |
| livenessProbe.httpGet.port | string | `"http"` |  |
| mas.captcha.secret_key | string | `""` | ignored if mas.captcha.service is ~ or null |
| mas.captcha.service | string | `nil` | Which service to use for CAPTCHA protection. Set to `null` (or `~`) to disable CAPTCHA protection. options are: ~, null, recaptcha_v2 (google), cloudflare_turnstile, or hcaptcha. see: https://matrix-org.github.io/matrix-authentication-service/reference/configuration.html#captcha |
| mas.captcha.site_key | string | `""` | ignored if mas.captcha.service is ~ or null |
| mas.clients[0] | object | `{"client_auth_method":"client_secret_basic","client_id":"0000000000000000000SYNAPSE","client_secret":"exampletest"}` | a unique identifier for the client. It must be a valid ULID, and it happens that 0000000000000000000SYNAPSE is a valid ULID. |
| mas.clients[0].client_auth_method | string | `"client_secret_basic"` | set to client_secret_basic. Other methods are possible, such as client_secret_post, but this is the easiest to set up. |
| mas.clients[0].client_secret | string | `"exampletest"` | a shared secret used for the homeserver to authenticate |
| mas.database.uri | string | `""` | specify a specific database uri to connect with |
| mas.email.command | string | `"/usr/sbin/sendmail"` | Send emails by calling a local sendmail binary, only used if transport is sendmail |
| mas.email.from | string | `"\"The almighty auth service\" <auth@example.com>"` | email.from |
| mas.email.hostname | string | `"localhost"` | SMTP hostname. only used if transport is smtp |
| mas.email.mode | string | `"plain"` | SMTP mode. options are plan, tls, or starttls. only used if transport is smtp |
| mas.email.password | string | `"password"` | SMTP password. only used if transport is smtp |
| mas.email.port | int | `587` | SMTP port. only used if transport is smtp |
| mas.email.reply_to | string | `"\"No reply\" <no-reply@example.com>"` | email.reply_to |
| mas.email.transport | string | `"blackhole"` | Default transport: don't send any emails, options: blackhole, smtp, sendmail, aws_ses (use AWS SESv2 API, via the AWS SDK, so the usual AWS environment variables are supported) |
| mas.email.username | string | `"username"` | SMTP username. only used if transport is smtp |
| mas.http.issuer | string | `""` | OIDC issuer advertised by the service. Defaults to `public_base` |
| mas.http.listeners | list | `[{"binds":[{"host":"localhost","port":8080}],"name":"web","proxy_protocol":false,"resources":[{"name":"discovery"},{"name":"human"},{"name":"oauth"},{"name":"compat"}],"tls":{}}]` | List of [HTTP listeners](https://matrix-org.github.io/matrix-authentication-service/reference/configuration.html#httplisteners) |
| mas.http.listeners[0].tls | object | `{}` | If set, makes the listener use TLS with the provided certificate and key. more info: https://matrix-org.github.io/matrix-authentication-service/reference/configuration.html#httplisteners |
| mas.http.public_base | string | `""` | Public URL base used when building absolute public URLs |
| mas.masClientSecret.existingSecret | string | `""` | use an existing secret for clients section of config.yaml for: mas.clients[0].client_id, mas.clients[0].client_secret if set, ignores mas.clients[0].client_id, mas.clients[0].client_secret |
| mas.masClientSecret.secretKeys.client_id | string | `"client_id"` | key in secret with the client_id |
| mas.masClientSecret.secretKeys.client_secret | string | `"client_secret"` | key in secret with the client_secret |
| mas.matrix.endpoint | string | `"https://localhost:8008"` | endpoint of your matrix home server (synapse or dendrite) with port if needed |
| mas.matrix.existingSecret | string | `""` | grab the above secret from an existing k8s secret. if set, ignores mas.matrix.secret |
| mas.matrix.homeserver | string | `"localhost:8008"` | name of your matrix home server (synapse or dendrite) with port if needed |
| mas.matrix.secret | string | `"test"` | a shared secret the service will use to call the homeserver admin API |
| mas.matrix.secretKey | string | `"secret"` | name of the key in existing secret to grab matrix.secret from |
| mas.passwords.enabled | bool | `false` | Whether to enable the password database. If disabled, users will only be able to log in using upstream OIDC providers |
| mas.policy.data.admin_clients | list | `[]` | Client IDs which are allowed to ask for admin access with a client_credentials grant |
| mas.policy.data.admin_users | list | `[]` | Users which are allowed to ask for admin access. If possible, use the can_request_admin flag on users instead. |
| mas.policy.data.client_registration | object | `{}` |  |
| mas.policy.data.passwords | object | `{}` |  |
| mas.upstream_oauth2.existingSecret | string | `""` | use an existing k8s secret for upstream oauth2 client_id and client_secret |
| mas.upstream_oauth2.providers[0] | object | `{"authorization_endpoint":"","brand_name":"","claims_imports":{"displayname":{"action":"suggest","template":"{{ user.name }}"},"email":{"action":"suggest","set_email_verification":"always","template":"{{ user.email }}"},"localpart":{"action":"require","template":"{{ user.preferred_username }}"},"subject":{"template":"{{ user.sub }}"}},"client_id":"","client_secret":"","discovery_mode":"oidc","human_name":"","id":"","issuer":"https://example.com/","jwks_uri":"","pkce_method":"auto","scope":"openid email profile","token_endpoint":"","token_endpoint_auth_method":"client_secret_basic"}` | A unique identifier for the provider Must be a valid ULID, and can be generated using online tools like: https://www.ulidtools.com |
| mas.upstream_oauth2.providers[0].authorization_endpoint | string | `""` | The provider authorization endpoint, takes precedence over the discovery mechanism |
| mas.upstream_oauth2.providers[0].brand_name | string | `""` | A brand identifier for the provider, which will be used to display a logo on the login page. Values supported by the default template are:  - `apple`  - `google`  - `facebook`  - `github`  - `gitlab`  - `twitter` |
| mas.upstream_oauth2.providers[0].claims_imports.displayname | object | `{"action":"suggest","template":"{{ user.name }}"}` | The display name is the user's display name. |
| mas.upstream_oauth2.providers[0].claims_imports.email | object | `{"action":"suggest","set_email_verification":"always","template":"{{ user.email }}"}` | An email address to import. |
| mas.upstream_oauth2.providers[0].claims_imports.email.set_email_verification | string | `"always"` | Whether the email address must be marked as verified. Possible values are:  - `import`: mark the email address as verified if the upstream provider     has marked it as verified, using the `email_verified` claim.     This is the default.   - `always`: mark the email address as verified   - `never`: mark the email address as not verified |
| mas.upstream_oauth2.providers[0].claims_imports.localpart | object | `{"action":"require","template":"{{ user.preferred_username }}"}` | The localpart is the local part of the user's Matrix ID. For example, on the `example.com` server, if the localpart is `alice`,  the user's Matrix ID will be `@alice:example.com`. |
| mas.upstream_oauth2.providers[0].claims_imports.subject | object | `{"template":"{{ user.sub }}"}` | The subject is an internal identifier used to link the user's provider identity to local accounts. By default it uses the `sub` claim as per the OIDC spec, which should fit most use cases. |
| mas.upstream_oauth2.providers[0].client_id | string | `""` | The client ID to use to authenticate to the provider |
| mas.upstream_oauth2.providers[0].client_secret | string | `""` | The client secret to use to authenticate to the provider This is only used by the `client_secret_post`, `client_secret_basic` and `client_secret_jwk` authentication methods |
| mas.upstream_oauth2.providers[0].discovery_mode | string | `"oidc"` | How the provider configuration and endpoints should be discovered |
| mas.upstream_oauth2.providers[0].human_name | string | `""` | A human-readable name for the provider, which will be displayed on the login page |
| mas.upstream_oauth2.providers[0].issuer | string | `"https://example.com/"` | The issuer URL, which will be used to discover the provider's configuration. If discovery is enabled, this *must* exactly match the `issuer` field advertised in `<issuer>/.well-known/openid-configuration`. |
| mas.upstream_oauth2.providers[0].jwks_uri | string | `""` | The provider JWKS URI. takes precedence over the discovery mechanism |
| mas.upstream_oauth2.providers[0].pkce_method | string | `"auto"` | Whether PKCE should be used during the authorization code flow. Possible values are:  - `auto`: use PKCE if the provider supports it (default)    Determined through discovery, and disabled if discovery is disabled  - `always`: always use PKCE (with the S256 method)  - `never`: never use PKCE |
| mas.upstream_oauth2.providers[0].scope | string | `"openid email profile"` | The scopes to request from the provider In most cases, it should always include `openid` scope |
| mas.upstream_oauth2.providers[0].token_endpoint | string | `""` | The provider token endpoint. takes precedence over the discovery mechanism |
| mas.upstream_oauth2.providers[0].token_endpoint_auth_method | string | `"client_secret_basic"` | Which authentication method to use to authenticate to the provider Supported methods are:   - `none`   - `client_secret_basic`   - `client_secret_post`   - `client_secret_jwt`   - `private_key_jwt` (using the keys defined in the `secrets.keys` section) |
| mas.upstream_oauth2.secretKeys.authorization_endpoint | string | `""` | key in secret with the authorization_endpoint if discovery is disabled |
| mas.upstream_oauth2.secretKeys.client_id | string | `"client_id"` | key in secret with the client_id |
| mas.upstream_oauth2.secretKeys.client_secret | string | `"client_secret"` | key in secret with the client_secret |
| mas.upstream_oauth2.secretKeys.id | string | `""` | key in secret with the provider id |
| mas.upstream_oauth2.secretKeys.issuer | string | `"issuer"` | key in secret with the issuer |
| mas.upstream_oauth2.secretKeys.token_endpoint | string | `""` | key in secret with the token_endpoint if discovery is disabled |
| mas.upstream_oauth2.secretKeys.userinfo_endpoint | string | `""` | key in secret with the userinfo_endpoint if discovery is disabled |
| nameOverride | string | `""` |  |
| networkPolicies.enabled | bool | `true` |  |
| nodeSelector | object | `{}` |  |
| podAnnotations | object | `{}` |  |
| podLabels | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| postgresql.enabled | bool | `false` | Whether to deploy the Bitnami Postgresql sub chart If postgresql.enabled is set to true, externalDatabase.enabled must be set to false else if externalDatabase.enabled is set to true, postgresql.enabled must be set to false |
| postgresql.global.postgresql.auth.database | string | `"mas"` | name of the database |
| postgresql.global.postgresql.auth.existingSecret | string | `""` | Name of existing secret to use for PostgreSQL credentials |
| postgresql.global.postgresql.auth.password | string | `"changeme"` | password of matrix-authentication-service postgres user - ignored using exsitingSecret |
| postgresql.global.postgresql.auth.port | string | `"5432"` | which port to use to connect to your database server |
| postgresql.global.postgresql.auth.secretKeys.adminPasswordKey | string | `"postgresPassword"` | key in existingSecret with the admin postgresql password |
| postgresql.global.postgresql.auth.secretKeys.database | string | `"database"` | key in existingSecret with name of the database |
| postgresql.global.postgresql.auth.secretKeys.databaseHostname | string | `"hostname"` | key in existingSecret with hostname of the database |
| postgresql.global.postgresql.auth.secretKeys.databaseUsername | string | `"username"` | key in existingSecret with username for matrix-authentication-service to connect to db |
| postgresql.global.postgresql.auth.secretKeys.userPasswordKey | string | `"password"` | key in existingSecret with password for matrix-authentication-service to connect to db |
| postgresql.global.postgresql.auth.username | string | `"mas"` | username of matrix-authentication-service postgres user |
| postgresql.primary.initdb | object | `{"scriptsConfigMap":"{{ .Release.Name }}-postgresql-initdb"}` | run the scripts in templates/postgresql/initdb-configmap.yaml If using an external Postgres server, make sure to configure the database ref: https://github.com/matrix-org/synapse/blob/master/docs/postgres.md |
| postgresql.primary.podSecurityContext.enabled | bool | `true` |  |
| postgresql.primary.podSecurityContext.fsGroup | int | `1000` |  |
| postgresql.primary.podSecurityContext.runAsUser | int | `1000` |  |
| postgresql.tls.autoGenerated | bool | `false` | Generate automatically self-signed TLS certificates |
| postgresql.tls.certCAFilename | string | `""` | CA Certificate filename |
| postgresql.tls.certFilename | string | `""` | Certificate filename |
| postgresql.tls.certKeyFilename | string | `""` | Certificate key filename |
| postgresql.tls.certificatesSecret | string | `""` | Name of an existing secret that contains the certificates |
| postgresql.tls.crlFilename | string | `""` | File containing a Certificate Revocation List |
| postgresql.tls.enabled | bool | `false` | Enable TLS traffic support for postgresql, see [bitnami/charts/postgresql#securing-traffic-using-tls](https://github.com/bitnami/charts/tree/main/bitnami/postgresql#securing-traffic-using-tls) |
| postgresql.tls.preferServerCiphers | bool | `true` | Whether to use the server's TLS cipher preferences rather than the client's |
| postgresql.volumePermissions.enabled | bool | `true` | Enable init container that changes the owner and group of the PVC |
| readinessProbe.enabled | bool | `false` | enable a readiness probe on the deployment |
| readinessProbe.httpGet.path | string | `"/"` |  |
| readinessProbe.httpGet.port | string | `"http"` |  |
| replicaCount | int | `1` |  |
| resources | object | `{}` | resources allocated to the kubernetes pod |
| securityContext | object | `{}` |  |
| service.annotations | object | `{}` | annotations for your service |
| service.port | int | `80` | Port of service |
| service.targetPort | int | `8080` | targetPort of service. should be the same as port for bindaddr |
| service.type | string | `"ClusterIP"` | type of service |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.automount | bool | `true` | Automatically mount a ServiceAccount's API credentials? |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| tolerations | list | `[]` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)
