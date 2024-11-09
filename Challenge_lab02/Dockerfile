FROM nginx:latest

# Xóa config mặc định
RUN rm /etc/nginx/conf.d/default.conf

# Copy config mới vào
COPY nginx-config/nginx.conf /etc/nginx/conf.d/

# Copy website tĩnh vào
COPY ../website /usr/share/nginx/html/

# Cài đặt permissions
RUN chmod -R 755 /usr/share/nginx/html