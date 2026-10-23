# Project 2: Design Rationale

## Why I Chose a Multi-Stage Docker Build

I used a multi-stage Dockerfile to separate building the application from running it. The first stage uses Node.js 24 to prepare the web application. The final stage uses nginx:alpine to serve the finished static files. This keeps unnecessary Node.js build tools out of the runtime image. Docker recommends multi-stage builds because they allow files to be copied from a build stage into a smaller final image.

I chose nginx:alpine because Nginx can efficiently serve static web pages, while Alpine provides a lightweight base. My local image comparison showed that the single-stage image was approximately 1.65 GB, compared with 93.6 MB for the multi-stage image. This is about a 94.3% reduction. A smaller image is easier to store and transfer and avoids shipping build dependencies that the web server does not need.

## Why Redis Is a Separate Service

My compose.yaml defines two services: web (Nginx) and cache (Redis). Redis runs separately because it has a different responsibility from the web server. Keeping services separate makes them easier to configure, replace, and manage independently. Redis includes a health check, and Docker Compose uses depends_on with the service_healthy condition so the web container starts only after Redis passes its health check.

I also tested stopping Redis while Nginx continued running. The website still returned HTTP 200 OK because this demonstration serves static files and does not currently depend on Redis for its response. This confirms that the containers have separate lifecycles, not that the application is using Redis for caching yet.

## Connection to Cloud Service Models

With Infrastructure as a Service (IaaS), the developer typically manages more of the operating system, runtime, and application environment. Platform as a Service (PaaS) handles more of that underlying environment. My local Docker Compose setup resembles a developer-managed service deployment: I configure the Nginx and Redis containers, their images, networking, and startup relationship. In a managed cloud platform, some of those operational responsibilities could be handled by the provider. AWS SAM adds a serverless example by letting me test a Lambda function locally without deploying it to AWS.

## References

1. Docker Docs. Multi-stage builds. https://docs.docker.com/build/building/multi-stage/
2. Docker Docs. Control startup and shutdown order in Compose. https://docs.docker.com/compose/how-tos/startup-order/
3. Docker Docs. Docker Compose overview. https://docs.docker.com/compose/
4. AWS Docs. Using sam local invoke. https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-local-invoke.html
5. Microsoft Azure. Shared responsibility in the cloud. https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility
