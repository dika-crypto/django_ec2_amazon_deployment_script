# Django EC2 Deployment Automation Script

![Deployment Automation](https://img.shields.io/badge/Deployment-Automation-brightgreen)
![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=flat&logo=nginx&logoColor=white)
![Let's Encrypt](https://img.shields.io/badge/Let's_Encrypt-003A70?style=flat&logo=letsencrypt&logoColor=white)

A comprehensive, interactive bash script that automates the deployment of Django applications on Amazon EC2 instances with Nginx, Gunicorn, PostgreSQL, and Let's Encrypt SSL certificates.

## 🚀 Features

- **One-Command Deployment**: Deploy your Django application with a single command
- **Automatic SSL Setup**: Integrated Let's Encrypt for free SSL certificates
- **Multiple Domain Support**: Configure multiple domains and subdomains
- **Production-Ready**: Includes Gunicorn, Nginx, and PostgreSQL configurations
- **Service Management**: Start, stop, restart, and check status of your application
- **Log Management**: Easy access to application and server logs
- **Automatic Updates**: Built-in update mechanism for your application
- **Security Best Practices**: Implements security headers and HTTPS enforcement

## 🛠 Prerequisites

- Ubuntu 20.04/22.04 LTS server (Amazon Linux 2 also supported)
- Python 3.8+
- Django 3.0+
- PostgreSQL (optional, can be configured during setup)
- Domain name (for SSL certificates)
- Sudo/root access to the server

## 🚀 Quick Start

1. **Copy the deployment script** to your server:
   ```bash
   wget https://raw.githubusercontent.com/yourusername/django_ec2_amazon_deployment_script/main/deploy_django.sh
   chmod +x deploy_django.sh
   ```

2. **Run the script** (as root or with sudo):
   ```bash
   sudo ./deploy_django.sh
   ```

3. **Follow the interactive prompts** to configure your deployment:
   - Enter your domain name and subdomains
   - Provide your email for SSL certificates
   - Specify your Django project path and name
   - Configure database settings
   - Set up additional options as needed

## 📂 Project Structure

After deployment, your project will have the following structure:

```
/var/www/
└── your_project/
    ├── app/                  # Your Django project
    ├── venv/                # Virtual environment
    ├── static/              # Static files
    ├── media/               # Media files
    ├── logs/                # Log files
    │   ├── gunicorn.log
    │   ├── nginx_access.log
    │   └── nginx_error.log
    ├── .env                 # Environment variables
    └── manage.py           # Django management script
```

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in your project root with the following variables:

```bash
# Django Settings
DEBUG=False
SECRET_KEY=your-secret-key
ALLOWED_HOSTS=.yourdomain.com,localhost

# Database Settings
DB_NAME=your_db_name
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_HOST=localhost
DB_PORT=5432

# Email Settings
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=your-email@gmail.com
EMAIL_HOST_PASSWORD=your-email-password

# Other Settings
DJANGO_SETTINGS_MODULE=your_project.settings
```

### Nginx Configuration

The script automatically configures Nginx with:
- Gzip compression
- Security headers
- Static and media file serving
- SSL redirection
- HTTP/2 support

### Gunicorn Configuration

Gunicorn is configured with:
- Multiple workers based on CPU cores
- Automatic worker restarts
- Logging to file
- Graceful reloading

## 🔄 Management Commands

After deployment, use these commands to manage your application:

```bash
# Start the application
sudo systemctl start your_project

# Stop the application
sudo systemctl stop your_project

# Restart the application
sudo systemctl restart your_project

# Check application status
sudo systemctl status your_project

# View application logs
sudo journalctl -u your_project -f

# Update application code
cd /var/www/your_project
sudo git pull origin main
sudo systemctl restart your_project
```

## 🔒 Security

- Automatic firewall configuration (UFW)
- Fail2Ban installation and configuration
- Automatic security updates
- Disabled root SSH login (recommended)
- Changed default SSH port (optional)

## 🔄 Updating Your Application

1. Push your changes to your Git repository
2. SSH into your server
3. Navigate to your project directory
4. Pull the latest changes
5. Run migrations and collect static files
6. Restart the application

```bash
cd /var/www/your_project
sudo -u www-data git pull
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py collectstatic --noinput
sudo systemctl restart your_project
```

## 🚨 Troubleshooting

### Common Issues

1. **Permission Denied**
   - Ensure the web server user (www-data) has proper permissions
   - Run: `sudo chown -R www-data:www-data /var/www/your_project`

2. **Port Already in Use**
   - Check running processes: `sudo lsof -i :80`
   - Stop the conflicting service or choose a different port

3. **SSL Certificate Issues**
   - Check certificate status: `sudo certbot certificates`
   - Renew certificates: `sudo certbot renew --dry-run`

4. **Database Connection Issues**
   - Verify database service is running: `sudo systemctl status postgresql`
   - Check connection settings in `.env`

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Contact

For questions or support, please open an issue on GitHub.

---

