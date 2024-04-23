# Builder - Used to build the application dependencies and assets
# REPOs default to DockerHub
ARG DOCKER_BASE_REPO=skillstream/rails-base
ARG DOCKER_REPO=skillstream/ssnext
ARG RAILS_BASE_VERSION
FROM $DOCKER_REPO:$RAILS_BASE_VERSION-builder as builder

# Copy the application code
COPY --chown=rails:rails . .

# Create the database configuration
COPY config/database.yml.production config/database.yml

ENV RAILS_ENV=production
ENV RAILS_LOG_TO_STDOUT=enabled

RUN ASSET_BUILD=1 rails assets:precompile

# Remove folders not needed in resulting image
# This includes `app/javascript` which contains the JavaScript source code.
# Normally it is not needed in the resulting image, because it was compiled
# to `public/`. But if the app uses importmaps, the folder is still required
# for pinning and must not be removed.
RUN rm -rf node_modules tmp/cache vendor/bundle test spec app/packs
RUN if [ ! -f config/importmap.rb ]; then rm -rf app/javascript; fi

# Instance image - This is the image that runs the application.
# This gets reset after the first FROM
ARG DOCKER_BASE_REPO=skillstream/rails-base
ARG RAILS_BASE_VERSION
FROM $DOCKER_BASE_REPO:$RAILS_BASE_VERSION

# Copy the runtime gems from former build stage
COPY --from=builder /usr/local/bundle/ /usr/local/bundle/

# Copy the application code from the former build process
COPY --from=builder --chown=rails:rails /app /app

ARG GIT_COMMIT_SHA
ENV GIT_COMMIT_SHA=$GIT_COMMIT_SHA
LABEL git.commit.sha=$GIT_COMMIT_SHA

ENV RAILS_ENV=production
ENV RAILS_LOG_TO_STDOUT=enabled

# Expose Puma port
EXPOSE 3000

# Run rails as the rails user
USER rails
CMD ["rails", "server", "-b", "0.0.0.0"]
