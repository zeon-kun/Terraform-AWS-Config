#AJK = Aku Jadi Keren
## Deploying Laravel with Nginx Configuration
on the root project directory, create a new .sh file, called 'command.sh'.
```
# Update installed dependencies
sudo apt-get update -y

# Install dependencies and add PHP8.0 repository
sudo apt-get install  ca-certificates apt-transport-https software-properties-common -y
sudo add-apt-repository ppa:ondrej/php -y

# Install nginx
sudo apt-get install nginx -y

# Set firewall permission for port 22, 80, and 443
echo "y" | sudo ufw enable
sudo ufw allow ssh
sudo ufw allow 'Nginx HTTP'
sudo ufw allow 443
sudo ufw allow 80
sudo ufw allow 22

# Install PHP8.0 version to run the Laravel Project
sudo apt-get update -y
sudo apt-get install php8.0-common php8.0-cli -y

# install PHP package
sudo apt-get install php8.0-mbstring php8.0-xml unzip composer -y
sudo apt-get install php8.0-curl php8.0-mysql php8.0-fpm -y

# install MYSQL
sudo apt-get install mysql-server -y

# Set MYSQL password
sudo apt-get install debconf-utils -y
debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
sudo debconf-set-selections <<< "mysql-server mysql-server/root_password_again password root"

# Nginx config
sudo cp /var/www/config/example.conf /etc/nginx/sites-available/example.conf

# Copy to sites-enabled directory
sudo ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled
sudo service nginx restart

# Clone the laravel template application
git clone https://gitlab.com/kuuhaku86/web-penugasan-individu.git


# Configurate the example.conf as below
server {
        listen 80;
        root /var/www/project/public;
        index index.php index.html index.htm index.nginx-debian.html;
        
        #Change this later to the dns when creating SSL via certbot
        server_name localhost;

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}


```
