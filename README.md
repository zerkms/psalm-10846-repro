# Readme

Psalm repro repository for the bug: https://github.com/vimeo/psalm/issues/10846

## How to run

```bash
docker build .
```

Output as of 2024-03-21:

```
➜ docker build .
[+] Building 8.2s (15/15) FINISHED                                                                                                                                                                                                                          docker:default
 => [internal] load build definition from Dockerfile                                                                                                                                                                                                                  0.0s
 => => transferring dockerfile: 327B                                                                                                                                                                                                                                  0.0s
 => [internal] load metadata for docker.io/library/composer:2                                                                                                                                                                                                         0.0s
 => [internal] load metadata for docker.io/library/php:8.3-cli                                                                                                                                                                                                        1.4s
 => [auth] library/php:pull token for registry-1.docker.io                                                                                                                                                                                                            0.0s
 => [internal] load .dockerignore                                                                                                                                                                                                                                     0.0s
 => => transferring context: 2B                                                                                                                                                                                                                                       0.0s
 => FROM docker.io/library/composer:2                                                                                                                                                                                                                                 0.0s
 => [internal] load build context                                                                                                                                                                                                                                     0.0s
 => => transferring context: 35.75kB                                                                                                                                                                                                                                  0.0s
 => [stage-0 1/8] FROM docker.io/library/php:8.3-cli@sha256:a02c99554f30eaa2db34c894cde57cf58da134c3b41d57b0277325e306217a08                                                                                                                                          0.0s
 => CACHED [stage-0 2/8] COPY --from=composer:2 /usr/bin/composer /usr/local/bin                                                                                                                                                                                      0.0s
 => CACHED [stage-0 3/8] RUN apt update && apt install -y --no-install-recommends unzip libicu-dev                                                                                                                                                                    0.0s
 => CACHED [stage-0 4/8] RUN docker-php-ext-install -j$(nproc) bcmath intl                                                                                                                                                                                            0.0s
 => CACHED [stage-0 5/8] WORKDIR /app-appname                                                                                                                                                                                                                         0.0s
 => [stage-0 6/8] COPY . /app-appname                                                                                                                                                                                                                                 0.1s
 => [stage-0 7/8] RUN composer install                                                                                                                                                                                                                                5.3s
 => ERROR [stage-0 8/8] RUN ./vendor/bin/psalm                                                                                                                                                                                                                        1.3s 
------                                                                                                                                                                                                                                                                     
 > [stage-0 8/8] RUN ./vendor/bin/psalm:                                                                                                                                                                                                                                   
0.341 Warning: "findUnusedBaselineEntry" will default to "true" in Psalm 6. You should explicitly enable or disable this setting.
0.341 Warning: "findUnusedCode" will default to "true" in Psalm 6. You should explicitly enable or disable this setting.
0.342 
0.342 Install the opcache extension to make use of JIT on PHP 8.0+ for a 20%+ performance boost!
0.342 
0.361 Target PHP version: 8.3 (inferred from current PHP version).
0.361 Scanning files...
1.223 Analyzing files...
1.223 
1.240 E
1.244 
1.244 ERROR: MissingOverrideAttribute - src/Test.php:8:5 - Method __toString should have the "Override" attribute (see https://psalm.dev/358)
1.244     public function __toString(): string
1.244 
1.244 
1.244 ------------------------------
1.244 1 errors found
1.244 ------------------------------
1.244 
1.244 Checks took 0.88 seconds and used 44.725MB of memory
1.244 Psalm was able to infer types for 100% of the codebase
------
Dockerfile:14
--------------------
  12 |     RUN composer install
  13 |     
  14 | >>> RUN ./vendor/bin/psalm
  15 |     
--------------------
ERROR: failed to solve: process "/bin/sh -c ./vendor/bin/psalm" did not complete successfully: exit code: 2
```
