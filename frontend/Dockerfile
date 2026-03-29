FROM nginx:alpine

# Remove default nginx page
RUN rm -rf /usr/share/nginx/html/*

# Copy all HTML files
COPY index.html /usr/share/nginx/html/
COPY add-book.html /usr/share/nginx/html/
COPY orders.html /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
