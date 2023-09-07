# Docker Best Practices

- **Multi-stage builds**: Breaking up Dockerfile into several stages makes images leaner and more secure. I didn't use it because:

  - My Dockerfile is straightforward and doesn't involve complex build processes or build-time dependencies that need to be separated from the final production image.

  - While multi-stage builds can help reduce the size of the final image, Python images provided by the official Python repository are already relatively small. The difference in image size in this case wouldn't be significant.

  - I am installing Python dependencies from requirements.txt in a single RUN instruction, which is a common practice. There's no need to separate the installation of dependencies into a separate stage because I am not performing complex build steps.

- **Trusted base images**: I've chosen to build upon the official Python 3.10 image from Docker Hub. Using official and well-maintained base images is a best practice because they are regularly updated with security patches, reducing the risk of vulnerabilities in my container.

- **Prefer COPY Over ADD**

- **Set a non-root user**

- **Prefer Array over String syntax**:

  ```
  # array
  CMD ["python", "app.py"]

  # string
  CMD "python app.py"
  ```

- **Order Dockerfile commands appropriately**: The order of Dockerfile commands is very important. Arrange Dockerfile instructions logically, starting with the base image (FROM) and following a sequential flow from essential system setup (RUN, USER, etc.) to application-specific tasks (COPY, CMD).

- **Minimize the number of layers**: Grouping multiple commands together will reduce the number of layers.

  ```
  RUN useradd -m myuser
  ...
  RUN chown -R myuser:myuser /app

  |
  V

  RUN useradd -m myuser && \
      mkdir /app && \
      chown myuser:myuser /app
  ```

# Linter for Dockerfile

[Hadolint](https://github.com/hadolint/hadolint) linter was used for Dockerfile.

Reasons to use it:

- Hadolint enforces consistent Dockerfile best practices, ensuring that all team members follow the same coding standards.
- Hadolint checks for security vulnerabilities and unsafe practices.
- It improves the readability of Dockerfiles by identifying and highlighting potential issues, making the Dockerfile easier to understand and maintain.
- It helps identify and fix issues early in the development process, reducing the time spent debugging and troubleshooting Docker images.