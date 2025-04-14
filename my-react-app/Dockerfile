# Use an official nginx image as the base image
FROM nginx:alpine

# Copy the build files to the nginx html directory
COPY build/ /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start nginx when the container launches
CMD ["nginx", "-g", "daemon off;"]
