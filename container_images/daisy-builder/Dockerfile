# Copyright 2018 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
FROM gcr.io/compute-image-tools/daisy:latest

FROM gcr.io/gcp-guest/autoversioner:latest

#FROM alpine
# Needed for gsutil.
FROM google/cloud-sdk:alpine

RUN apk add --no-cache bash
# Should this be set by the prow config? Creds are provided by it.
ENV GOOGLE_APPLICATION_CREDENTIALS /etc/daisy-service-account/creds.json
ENV GITHUB_ACCESS_TOKEN /etc/github-access-token/token.txt

WORKDIR /
COPY --from=0 /daisy daisy
# Daisy image copies this from gcr.io/distroless/base, we copy here.
COPY --from=0 /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt

COPY --from=1 /tagger tagger
COPY --from=1 /versiongenerator versiongenerator

# Copy this Dockerfile for debugging.
COPY Dockerfile Dockerfile
COPY main.sh main.sh

ENTRYPOINT ["./main.sh"]