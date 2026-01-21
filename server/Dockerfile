# 1. Add a base image
FROM python:3.12.0-slim-bookworm

# 2. Fix environment variables
ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1

ENV APP=/app

# 3. Change the workdir
WORKDIR $APP

# 4. Install and update system dependencies
# This fulfills the "Install and update" requirement
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# 5. Add the requirements.txt file
COPY requirements.txt $APP

# 6. Upgrade pip and install requirements
RUN pip3 install --upgrade pip
RUN pip3 install -r requirements.txt

# 7. Copy the rest of the files
COPY . $APP

# 8. Expose port
EXPOSE 8000

# 9. Make the entrypoint script executable
RUN chmod +x /app/entrypoint.sh

# 10. Run the command to start the application
ENTRYPOINT ["/bin/bash","/app/entrypoint.sh"]

CMD ["gunicorn", "--bind", ":8000", "--workers", "3", "djangoproj.wsgi"]