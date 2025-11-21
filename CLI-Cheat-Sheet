# Full-Stack Developer Linux Cheat Sheet

## 🚀 Quick Navigation
- [Navigation & File Operations](#navigation--file-operations)
- [File Search & Content](#file-search--content)
- [Process Management](#process-management)
- [Network & API Testing](#network--api-testing)
- [Git Operations](#git-operations)
- [Node.js & npm](#nodejs--npm)
- [Docker](#docker)
- [Database Operations](#database-operations)
- [Server Management](#server-management)
- [System Monitoring](#system-monitoring)
- [Environment & Variables](#environment--variables)
- [SSH & Remote](#ssh--remote)
- [Log Management](#log-management)
- [Build & Deploy](#build--deploy)
- [Debugging](#debugging)
- [Productivity Hacks](#productivity-hacks)

---

## Navigation & File Operations

### Basic Navigation
```bash
pwd                          # Print working directory
cd /path/to/directory       # Change directory
cd ~                        # Go to home directory
cd -                        # Go to previous directory
cd ..                       # Go up one level
cd ../..                    # Go up two levels
```

### Listing Files
```bash
ls                          # List files
ls -la                      # List all files with details
ls -lh                      # Human-readable file sizes
ls -lt                      # Sort by modification time
ls -lS                      # Sort by size
ls -R                       # Recursive listing
tree                        # Tree view of directories
tree -L 2                   # Tree with depth limit
tree -I 'node_modules|dist' # Exclude directories
```

### File Operations
```bash
# Create
touch file.txt              # Create empty file
mkdir project               # Create directory
mkdir -p path/to/dir        # Create nested directories

# Copy
cp file.txt backup.txt      # Copy file
cp -r src/ backup/          # Copy directory recursively
cp -v file.txt dest/        # Verbose copy

# Move/Rename
mv old.txt new.txt          # Rename file
mv file.txt /path/to/       # Move file
mv *.js scripts/            # Move all JS files

# Delete
rm file.txt                 # Delete file
rm -f file.txt              # Force delete
rm -r directory/            # Delete directory recursively
rm -rf node_modules/        # Force delete (be careful!)
find . -name "*.log" -delete # Delete all .log files
```

### Quick File Operations
```bash
# Create multiple files
touch {file1,file2,file3}.txt
touch index.{html,css,js}

# Create directory structure
mkdir -p src/{components,utils,services,assets}

# Backup with timestamp
cp app.js app.js.$(date +%Y%m%d_%H%M%S).bak

# Quick file content
cat > file.txt              # Create and write (Ctrl+D to save)
cat file.txt                # Display file content
cat file1.txt file2.txt > combined.txt # Combine files
```

---

## File Search & Content

### Find Files
```bash
# Find by name
find . -name "*.js"         # Find all JS files
find . -iname "*.JS"        # Case-insensitive
find . -name "index.*"      # Find by pattern
find /var/log -name "*.log" -mtime -7 # Modified in last 7 days

# Find by type
find . -type f              # Files only
find . -type d              # Directories only
find . -type f -name "*.json" # JSON files

# Find by size
find . -size +10M           # Larger than 10MB
find . -size -1k            # Smaller than 1KB
find . -empty               # Empty files

# Find and execute
find . -name "*.tmp" -delete
find . -name "*.js" -exec grep "TODO" {} \;
find . -type f -name "*.log" -exec rm {} \;

# Modern alternative: fd (faster)
fd "\.js$"                  # Find JS files
fd -e py                    # Find Python files
fd -H node_modules          # Include hidden files
```

### Search Content (grep)
```bash
# Basic search
grep "error" app.log        # Search in file
grep -r "TODO" .            # Recursive search
grep -ri "error" .          # Case-insensitive recursive

# Advanced search
grep -n "function" app.js   # Show line numbers
grep -v "debug" app.log     # Invert match (exclude)
grep -c "error" app.log     # Count matches
grep -l "import" *.js       # Show filenames only
grep -A 3 "error" app.log   # Show 3 lines after match
grep -B 2 "error" app.log   # Show 2 lines before match
grep -C 2 "error" app.log   # Show 2 lines context

# Multiple patterns
grep -E "error|warning" app.log
grep "error" app.log | grep -v "debug"

# Modern alternative: ripgrep (rg) - much faster
rg "TODO"                   # Smart recursive search
rg "error" -t js            # Search in JS files only
rg "API_KEY" --hidden       # Include hidden files
rg "password" -g "!*.min.js" # Exclude minified files
```

### Find and Replace
```bash
# Replace in file
sed -i 's/old/new/g' file.txt
sed -i 's/localhost/production.com/g' config.js

# Replace in multiple files
find . -name "*.js" -exec sed -i 's/var/const/g' {} \;

# Replace with backup
sed -i.bak 's/old/new/g' file.txt

# Modern alternative: sd
sd 'old' 'new' file.txt
fd -e js -x sd 'var' 'const'
```

---

## Process Management

### View Processes
```bash
ps                          # Current shell processes
ps aux                      # All processes
ps aux | grep node          # Find Node processes
ps -ef | grep python        # Find Python processes
pgrep -a node               # Find process by name

# Better process viewer
top                         # Interactive process viewer
htop                        # Better than top (install: sudo apt install htop)

# Press in htop:
# F3 - search, F4 - filter, F5 - tree view, F6 - sort, F9 - kill
```

### Kill Processes
```bash
# Kill by PID
kill 1234                   # Graceful termination
kill -9 1234                # Force kill
kill -15 1234               # SIGTERM (same as kill)

# Kill by name
pkill node                  # Kill all Node processes
pkill -f "node server.js"   # Kill specific Node process
killall node                # Kill all Node processes

# Find and kill
ps aux | grep node | awk '{print $2}' | xargs kill

# Kill process on specific port
lsof -ti:3000 | xargs kill
lsof -ti:8080 | xargs kill -9

# Kill all Node processes
pkill -9 node
```

### Background Jobs
```bash
# Run in background
npm start &                 # Run in background
node server.js &

# Job control
jobs                        # List background jobs
fg                          # Bring to foreground
bg                          # Resume in background
Ctrl+Z                      # Suspend current job

# Disown (continue after logout)
nohup npm start &           # Keep running after logout
nohup node server.js > output.log 2>&1 &

# Better alternative: pm2
pm2 start server.js
pm2 list
pm2 stop server
pm2 restart server
pm2 logs server
```

---

## Network & API Testing

### Check Ports
```bash
# Check if port is in use
lsof -i :3000               # Check port 3000
lsof -i :8080               # Check port 8080
netstat -tulpn | grep :3000 # Alternative
ss -tulpn | grep :3000      # Modern alternative

# Find what's using a port
sudo lsof -i :80            # What's on port 80
sudo fuser 3000/tcp         # Process using port 3000

# Kill process on port
kill -9 $(lsof -ti:3000)
```

### curl (API Testing)
```bash
# GET request
curl http://localhost:3000/api/users
curl -i http://localhost:3000  # Include headers

# POST request
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}'

# PUT request
curl -X PUT http://localhost:3000/api/users/123 \
  -H "Content-Type: application/json" \
  -d '{"name":"John Updated"}'

# DELETE request
curl -X DELETE http://localhost:3000/api/users/123

# With authentication
curl -H "Authorization: Bearer TOKEN" \
  http://localhost:3000/api/protected

# Save response to file
curl http://api.example.com/data > response.json
curl -o response.json http://api.example.com/data

# Follow redirects
curl -L http://example.com

# Verbose output (debugging)
curl -v http://localhost:3000
curl -i http://localhost:3000  # Include response headers

# Test with query parameters
curl "http://localhost:3000/api/users?page=1&limit=10"

# Upload file
curl -F "file=@upload.jpg" http://localhost:3000/upload
```

### wget
```bash
# Download file
wget https://example.com/file.zip
wget -O custom-name.zip https://example.com/file.zip

# Download recursively
wget -r https://example.com/docs/

# Continue interrupted download
wget -c https://example.com/large-file.iso
```

### Network Testing
```bash
# Check connectivity
ping google.com
ping -c 4 localhost          # Send 4 packets

# DNS lookup
nslookup example.com
dig example.com
host example.com

# Test port connectivity
telnet localhost 3000
nc -zv localhost 3000        # Check if port is open

# Check network interfaces
ifconfig                     # Network interfaces
ip addr show                 # Modern alternative
ip route                     # Routing table
```

---

## Git Operations

### Basic Git
```bash
# Setup
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --list

# Initialize
git init
git clone https://github.com/user/repo.git
git clone git@github.com:user/repo.git  # SSH

# Status & Info
git status
git status -s               # Short format
git log
git log --oneline           # Compact log
git log --graph --oneline --all
git diff                    # Unstaged changes
git diff --staged           # Staged changes
git show HEAD               # Last commit details
```

### Staging & Committing
```bash
# Stage files
git add .                   # Add all files
git add *.js                # Add all JS files
git add src/                # Add directory
git add -p                  # Interactive staging

# Unstage
git reset HEAD file.txt     # Unstage file
git restore --staged file.txt # Modern way

# Commit
git commit -m "Add feature"
git commit -am "Quick commit" # Stage and commit tracked files
git commit --amend          # Modify last commit

# Undo changes
git checkout -- file.txt    # Discard changes
git restore file.txt        # Modern way
git reset --hard HEAD       # Discard all changes
```

### Branches
```bash
# List branches
git branch                  # Local branches
git branch -r               # Remote branches
git branch -a               # All branches

# Create & switch
git branch feature-login
git checkout feature-login
git checkout -b feature-login  # Create and switch
git switch -c feature-login    # Modern way

# Merge
git checkout main
git merge feature-login
git merge --no-ff feature-login  # No fast-forward

# Delete branch
git branch -d feature-login    # Safe delete
git branch -D feature-login    # Force delete
git push origin --delete feature-login  # Delete remote
```

### Remote Operations
```bash
# View remotes
git remote -v
git remote show origin

# Fetch & Pull
git fetch origin
git pull origin main
git pull --rebase           # Rebase instead of merge

# Push
git push origin main
git push -u origin feature  # Set upstream
git push --force            # Force push (careful!)
git push --force-with-lease # Safer force push

# Sync fork
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git merge upstream/main
```

### Useful Git Commands
```bash
# Stash changes
git stash                   # Save changes
git stash list              # List stashes
git stash pop               # Apply last stash
git stash apply             # Apply without removing
git stash drop              # Delete last stash
git stash clear             # Delete all stashes

# Cherry pick
git cherry-pick abc123      # Apply specific commit

# Rebase
git rebase main             # Rebase current branch
git rebase -i HEAD~3        # Interactive rebase last 3 commits

# Clean
git clean -n                # Preview files to delete
git clean -fd               # Delete untracked files and directories

# Undo commits
git reset --soft HEAD~1     # Undo commit, keep changes
git reset --hard HEAD~1     # Undo commit, discard changes
git revert abc123           # Create new commit that undoes changes

# Search
git log -S "function name"  # Find commits with text
git log --grep "bug fix"    # Search commit messages
git blame file.txt          # Who changed what

# Shortcuts
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --all"
```

---

## Node.js & npm

### npm Basics
```bash
# Initialize project
npm init
npm init -y                 # Quick init with defaults

# Install packages
npm install express         # Install and save to dependencies
npm i express               # Short version
npm install -D jest         # Install as dev dependency
npm install -g nodemon      # Install globally

# Install from package.json
npm install
npm ci                      # Clean install (faster for CI/CD)

# Update packages
npm outdated                # Check outdated packages
npm update                  # Update packages
npm update express          # Update specific package

# Remove packages
npm uninstall express
npm uninstall -D jest
npm uninstall -g nodemon
```

### Running Scripts
```bash
# Run scripts from package.json
npm start
npm test
npm run build
npm run dev
npm run lint

# List available scripts
npm run

# Run with arguments
npm run build -- --production
npm test -- --coverage
```

### Package Management
```bash
# View package info
npm list                    # List installed packages
npm list --depth=0          # Top-level only
npm list -g --depth=0       # Global packages
npm view express            # Package info
npm view express versions   # All versions

# Cache
npm cache verify
npm cache clean --force

# Check for security issues
npm audit
npm audit fix
npm audit fix --force

# Package versions
npm version patch           # Bump patch version (1.0.0 → 1.0.1)
npm version minor           # Bump minor version (1.0.0 → 1.1.0)
npm version major           # Bump major version (1.0.0 → 2.0.0)
```

### npx (Execute packages)
```bash
npx create-react-app my-app
npx create-next-app my-app
npx express-generator my-api
npx prettier --write .
npx eslint .
```

### yarn (Alternative to npm)
```bash
yarn                        # Install dependencies
yarn add express            # Install package
yarn add -D jest            # Install dev dependency
yarn remove express         # Remove package
yarn upgrade                # Update packages
yarn global add nodemon     # Install globally
```

### pnpm (Fast alternative)
```bash
pnpm install
pnpm add express
pnpm remove express
pnpm update
```

---

## Docker

### Container Basics
```bash
# Run containers
docker run nginx            # Run container
docker run -d nginx         # Run in background
docker run -d -p 8080:80 nginx  # Map ports
docker run -d --name my-nginx nginx  # Name container
docker run -it ubuntu bash  # Interactive terminal

# List containers
docker ps                   # Running containers
docker ps -a                # All containers
docker ps -q                # Container IDs only

# Stop & start
docker stop container_id
docker stop $(docker ps -q) # Stop all running
docker start container_id
docker restart container_id

# Remove containers
docker rm container_id
docker rm -f container_id   # Force remove running
docker rm $(docker ps -aq)  # Remove all stopped
docker container prune      # Remove all stopped containers
```

### Images
```bash
# List images
docker images
docker images -q            # Image IDs only

# Pull & push
docker pull nginx:latest
docker pull node:18-alpine
docker push username/image:tag

# Build
docker build -t myapp:latest .
docker build -t myapp:v1.0 .
docker build --no-cache -t myapp .

# Remove images
docker rmi image_id
docker rmi $(docker images -q)  # Remove all
docker image prune          # Remove dangling images
docker image prune -a       # Remove unused images
```

### Docker Compose
```bash
# Start services
docker-compose up           # Start and view logs
docker-compose up -d        # Start in background
docker-compose up --build   # Rebuild and start

# Stop services
docker-compose down         # Stop and remove containers
docker-compose down -v      # Also remove volumes
docker-compose stop         # Stop without removing

# View logs
docker-compose logs
docker-compose logs -f      # Follow logs
docker-compose logs app     # Logs for specific service

# Execute commands
docker-compose exec app bash
docker-compose exec db psql -U postgres

# Restart services
docker-compose restart
docker-compose restart app

# Scale services
docker-compose up -d --scale app=3
```

### Docker Utilities
```bash
# Inspect
docker inspect container_id
docker logs container_id
docker logs -f container_id # Follow logs
docker logs --tail 100 container_id  # Last 100 lines

# Execute commands
docker exec -it container_id bash
docker exec -it container_id sh
docker exec container_id npm install

# Copy files
docker cp file.txt container_id:/app/
docker cp container_id:/app/file.txt ./

# Container stats
docker stats                # Real-time stats
docker stats container_id

# Clean up
docker system prune         # Remove unused data
docker system prune -a      # Remove all unused data
docker volume prune         # Remove unused volumes

# Networks
docker network ls
docker network create my-network
docker network rm my-network
```

### Dockerfile Quick Reference
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

---

## Database Operations

### PostgreSQL
```bash
# Connect to database
psql -U postgres
psql -U username -d database_name
psql -h localhost -p 5432 -U postgres -d mydb

# Inside psql
\l                          # List databases
\c database_name            # Connect to database
\dt                         # List tables
\d table_name               # Describe table
\du                         # List users
\q                          # Quit

# Execute SQL file
psql -U postgres -d mydb -f script.sql

# Backup & Restore
pg_dump mydb > backup.sql
pg_dump -U postgres mydb > backup.sql
psql -U postgres mydb < backup.sql

# Create database
createdb mydb
dropdb mydb
```

### MySQL/MariaDB
```bash
# Connect
mysql -u root -p
mysql -u username -p database_name
mysql -h localhost -u root -p

# Inside mysql
SHOW DATABASES;
USE database_name;
SHOW TABLES;
DESCRIBE table_name;
SELECT * FROM users;
EXIT;

# Execute SQL file
mysql -u root -p database_name < script.sql

# Backup & Restore
mysqldump -u root -p database_name > backup.sql
mysql -u root -p database_name < backup.sql

# Create database
mysql -u root -p -e "CREATE DATABASE mydb;"
```

### MongoDB
```bash
# Connect
mongo
mongosh                     # New shell

# Inside mongosh
show dbs                    # List databases
use mydb                    # Switch database
show collections            # List collections
db.users.find()             # Query collection
db.users.find().pretty()    # Pretty print
exit

# Import/Export
mongodump --db mydb --out /backup/
mongorestore --db mydb /backup/mydb/
mongoexport --db mydb --collection users --out users.json
mongoimport --db mydb --collection users --file users.json
```

### Redis
```bash
# Connect
redis-cli
redis-cli -h localhost -p 6379

# Inside redis-cli
PING                        # Test connection
KEYS *                      # List all keys
GET key                     # Get value
SET key value               # Set value
DEL key                     # Delete key
FLUSHALL                    # Clear all data
exit

# Execute commands
redis-cli SET mykey "value"
redis-cli GET mykey
```

---

## Server Management

### Service Management (systemd)
```bash
# Start/Stop services
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx

# Enable/Disable on boot
sudo systemctl enable nginx
sudo systemctl disable nginx

# Status
sudo systemctl status nginx
sudo systemctl is-active nginx
sudo systemctl is-enabled nginx

# List all services
sudo systemctl list-units --type=service
sudo systemctl list-unit-files --type=service
```

### Nginx
```bash
# Test configuration
sudo nginx -t

# Reload configuration
sudo nginx -s reload
sudo systemctl reload nginx

# View logs
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log

# Configuration location
/etc/nginx/nginx.conf
/etc/nginx/sites-available/
/etc/nginx/sites-enabled/
```

### Apache
```bash
# Test configuration
sudo apache2ctl configtest
sudo apachectl -t

# Reload
sudo systemctl reload apache2

# Logs
sudo tail -f /var/log/apache2/access.log
sudo tail -f /var/log/apache2/error.log

# Enable/disable sites
sudo a2ensite mysite.conf
sudo a2dissite mysite.conf
sudo systemctl reload apache2
```

### SSL/TLS Certificates
```bash
# Let's Encrypt with Certbot
sudo certbot --nginx -d example.com
sudo certbot --apache -d example.com
sudo certbot renew --dry-run
sudo certbot certificates

# Manual renewal
sudo certbot renew
```

---

## System Monitoring

### Disk Usage
```bash
# Disk space
df -h                       # Human-readable disk space
df -h /var/www              # Specific directory
du -sh *                    # Size of current directory contents
du -sh /var/log/*           # Size of log files
du -h --max-depth=1 /var/www  # One level deep

# Find large files
find / -type f -size +100M 2>/dev/null
find . -type f -size +10M -exec ls -lh {} \;

# Sort by size
du -h /var/log | sort -rh | head -20

# Check inode usage
df -i
```

### Memory Usage
```bash
# Memory info
free -h                     # Human-readable
free -m                     # In megabytes
cat /proc/meminfo

# Swap usage
swapon --show
```

### CPU & Load
```bash
# CPU info
lscpu
cat /proc/cpuinfo
nproc                       # Number of cores

# System load
uptime
w
top
htop
```

### Real-time Monitoring
```bash
# Watch command (repeat every 2 seconds)
watch -n 2 'df -h'
watch -n 1 'docker ps'
watch -n 5 'free -h'

# Monitor file changes
tail -f /var/log/syslog
tail -f app.log | grep ERROR

# Network monitoring
sudo iftop                  # Network usage by process
sudo nethogs                # Network usage per process
sudo tcpdump -i eth0        # Packet capture
```

---

## Environment & Variables

### Environment Variables
```bash
# View variables
env                         # All environment variables
echo $PATH
echo $HOME
echo $USER
printenv NODE_ENV

# Set temporarily
export NODE_ENV=production
export API_KEY="your-key"

# Set for command
NODE_ENV=production npm start
PORT=8080 node server.js

# Unset
unset NODE_ENV
```

### .env Files
```bash
# Create .env file
cat > .env << EOF
NODE_ENV=development
DATABASE_URL=postgresql://localhost:5432/mydb
API_KEY=secret123
PORT=3000
EOF

# Load .env in Node.js (with dotenv package)
# require('dotenv').config()

# View .env
cat .env
cat .env | grep API
```

### Shell Configuration
```bash
# Edit bash profile
nano ~/.bashrc              # For bash
nano ~/.zshrc               # For zsh

# Add to end of file:
export NODE_ENV=development
export PATH=$PATH:/usr/local/bin
alias gs='git status'

# Reload configuration
source ~/.bashrc
source ~/.zshrc

# View current shell
echo $SHELL
```

---

## SSH & Remote

### SSH Connection
```bash
# Connect to server
ssh user@server.com
ssh user@192.168.1.100
ssh -p 2222 user@server.com # Custom port

# With identity file
ssh -i ~/.ssh/id_rsa user@server.com

# Execute command remotely
ssh user@server.com 'ls -la /var/www'
ssh user@server.com 'docker ps'
```

### SSH Keys
```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096 -C "your@email.com"
ssh-keygen -t ed25519 -C "your@email.com"  # Modern

# Copy key to server
ssh-copy-id user@server.com
cat ~/.ssh/id_rsa.pub | ssh user@server.com 'cat >> ~/.ssh/authorized_keys'

# Test connection
ssh -T git@github.com
```

### SCP (Copy files over SSH)
```bash
# Copy to server
scp file.txt user@server.com:/home/user/
scp -r directory/ user@server.com:/var/www/

# Copy from server
scp user@server.com:/var/log/app.log ./
scp -r user@server.com:/var/www/ ./backup/

# With custom port
scp -P 2222 file.txt user@server.com:/path/
```

### rsync (Better than scp)
```bash
# Sync files to server
rsync -avz /local/path/ user@server.com:/remote/path/

# Sync from server
rsync -avz user@server.com:/remote/path/ /local/path/

# Exclude files
rsync -avz --exclude 'node_modules' --exclude '.git' \
  /local/path/ user@server.com:/remote/path/

# Dry run (preview)
rsync -avz --dry-run /local/path/ user@server.com:/remote/path/

# Delete files on destination that don't exist in source
rsync -avz --delete /local/path/ user@server.com:/remote/path/
```

### SSH Config
```bash
# Edit SSH config
nano ~/.ssh/config

# Add configuration:
Host myserver
    HostName server.com
    User username
    Port 22
    IdentityFile ~/.ssh/id_rsa

Host github
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_rsa

# Now connect with:
ssh myserver
```

---

## Log Management

### View Logs
```bash
# System logs
sudo tail -f /var/log/syslog
sudo tail -f /var/log/auth.log
sudo journalctl -f          # Follow systemd logs

# Application logs
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
tail -f app.log

# Last N lines
tail -n 100 app.log
head -n 50 app.log

# View with pagination
less /var/log/syslog
more /var/log/syslog
```

### Search Logs
```bash
# Search for errors
grep "ERROR" app.log
grep -i "error" app.log     # Case-insensitive
grep -v "debug" app.log     # Exclude debug

# Count occurrences
grep -c "ERROR" app.log

# Search with context
grep -A 5 "ERROR" app.log   # 5 lines after
grep -B 5 "ERROR" app.log   # 5 lines before
grep -C 5 "ERROR" app.log   # 5 lines context

# Multiple log files
grep "ERROR" *.log
grep -r "ERROR" /var/log/

# Time-based search (with awk)
awk '/2024-11-21.*ERROR/' app.log
```

### Log Rotation
```bash
# Compress old logs
gzip app.log.1
gzip /var/log/nginx/*.log.1

# View compressed logs
zcat app.log.gz
zless app.log.gz
zgrep "ERROR" app.log.gz
```

### journalctl (systemd logs)
```bash
# View logs
sudo journalctl
sudo journalctl -f          # Follow
sudo journalctl -n 100      # Last 100 lines

# Filter by service
sudo journalctl -u nginx
sudo journalctl -u nginx -f

# Filter by time
sudo journalctl --since "1 hour ago"
sudo journalctl --since "2024-11-21"
sudo journalctl --since "2024-11-21 10:00" --until "2024-11-21 11:00"

# Filter by priority
sudo journalctl -p err      # Errors only
sudo journalctl -p warning  # Warnings and above
```

---

## Build & Deploy

### Build Frontend
```bash
# React
npm run build
npx react-scripts build

# Vue
npm run build

# Angular
ng build --prod

# Next.js
npm run build
npm run start

# Vite
npm run build
npm run preview
```

### Deploy
```bash
# Deploy to server via rsync
rsync -avz --delete dist/ user@server.com:/var/www/html/

# Deploy via SCP
scp -r dist/* user@server.com:/var/www/html/

# Deploy with Git (on server)
git pull origin main
npm install
npm run build
pm2 restart app
```

### PM2 (Process Manager)
```bash
# Start application
pm2 start server.js
pm2 start npm --name "my-app" -- start
pm2 start server.js --name api --instances 4  # Cluster mode

# Manage
pm2 list                    # List all processes
pm2 stop app_name
pm2 restart app_name
pm2 reload app_name         # Zero-downtime reload
pm2 delete app_name

# Logs
pm2 logs                    # All logs
pm2 logs app_name           # Specific app
pm2 logs --lines 200

# Monitoring
pm2 monit                   # Real-time monitoring
pm2 status

# Startup script
pm2 startup                 # Generate startup script
pm2 save                    # Save current process list
pm2 resurrect               # Restore saved processes

# Update PM2
pm2 update

# Ecosystem file (pm2.config.js)
pm2 start pm2.config.js
pm2 restart pm2.config.js
```

### Example pm2.config.js
```javascript
module.exports = {
  apps: [{
    name: 'api',
    script: './server.js',
    instances: 2,
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'development',
      PORT: 3000
    },
    env_production: {
      NODE_ENV: 'production',
      PORT: 8080
    }
  }]
}
```

---

## Debugging

### Check Server Status
```bash
# Is service running?
sudo systemctl status nginx
ps aux | grep nginx
pgrep -a nginx

# Is port open?
lsof -i :3000
netstat -tulpn | grep :3000
ss -tulpn | grep :3000

# Test endpoint
curl -I http://localhost:3000
curl -v http://localhost:3000
wget --spider http://localhost:3000
```

### Network Debugging
```bash
# Check DNS
nslookup example.com
dig example.com
host example.com

# Trace route
traceroute example.com
mtr example.com             # Better traceroute

# Test port connectivity
telnet example.com 80
nc -zv example.com 80
timeout 5 bash -c "</dev/tcp/example.com/80" && echo "Port open"

# Check firewall
sudo ufw status
sudo iptables -L
```

### Application Debugging
```bash
# Check logs for errors
grep -i error /var/log/nginx/error.log
tail -f app.log | grep -i error

# Check disk space (common issue)
df -h
du -sh /var/www/*

# Check memory
free -h
top -o %MEM                 # Sort by memory

# Check open files limit
ulimit -a
ulimit -n                   # Max open files

# List open files by process
lsof -p PID
lsof | grep node

# Environment check
printenv
echo $NODE_ENV
echo $PATH
```

### Performance Debugging
```bash
# CPU usage
top -o %CPU
ps aux --sort=-%cpu | head

# Memory usage
ps aux --sort=-%mem | head

# I/O stats
iostat
iotop                       # Real-time I/O

# Network connections
netstat -an | grep ESTABLISHED
ss -s                       # Socket statistics

# Trace system calls
strace -p PID
strace node server.js

# Profile Node.js
node --prof server.js
node --prof-process isolate-*.log

# Check for zombie processes
ps aux | grep 'Z'
```

---

## Productivity Hacks

### Command Shortcuts
```bash
# Navigation
cd -                        # Previous directory
pushd /path && popd         # Directory stack

# History
!!                          # Repeat last command
!$                          # Last argument of previous command
!ssh                        # Last command starting with 'ssh'
Ctrl+R                      # Search command history

# Quick edits
fc                          # Edit last command in editor
Ctrl+X Ctrl+E              # Edit command in editor
```

### Aliases (Add to ~/.bashrc or ~/.zshrc)
```bash
# Navigation
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'
alias home='cd ~'

# Git shortcuts
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git log --oneline --graph'
alias gd='git diff'
alias gb='git branch'
alias gco='git checkout'

# Docker shortcuts
alias dps='docker ps'
alias dpsa='docker ps -a'
alias di='docker images'
alias dstop='docker stop $(docker ps -q)'
alias drm='docker rm $(docker ps -aq)'
alias drmi='docker rmi $(docker images -q)'
alias dcu='docker-compose up -d'
alias dcd='docker-compose down'
alias dcl='docker-compose logs -f'

# npm shortcuts
alias ni='npm install'
alias nid='npm install -D'
alias nig='npm install -g'
alias nr='npm run'
alias ns='npm start'
alias nt='npm test'
alias nb='npm run build'

# System shortcuts
alias ll='ls -lah'
alias la='ls -A'
alias l='ls -CF'
alias ports='lsof -i -P -n | grep LISTEN'
alias meminfo='free -h'
alias cpuinfo='lscpu'
alias diskinfo='df -h'

# Quick servers
alias serve='python3 -m http.server 8000'
alias phpserve='php -S localhost:8000'

# Process management
alias killnode='pkill -9 node'
alias killport='kill -9 $(lsof -ti:3000)'

# Networking
alias myip='curl ifconfig.me'
alias localip='ip addr show | grep inet'
alias ports='netstat -tulanp'

# Safety
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Quick navigation to common directories
alias www='cd /var/www'
alias logs='cd /var/log'
alias proj='cd ~/projects'

# Reload shell config
alias reload='source ~/.bashrc'  # or ~/.zshrc
```

### Functions (Add to ~/.bashrc or ~/.zshrc)
```bash
# Create and enter directory
mkcd() {
  mkdir -p "$1" && cd "$1"
}

# Extract any archive
extract() {
  if [ -f $1 ]; then
    case $1 in
      *.tar.bz2)   tar xjf $1     ;;
      *.tar.gz)    tar xzf $1     ;;
      *.bz2)       bunzip2 $1     ;;
      *.rar)       unrar e $1     ;;
      *.gz)        gunzip $1      ;;
      *.tar)       tar xf $1      ;;
      *.tbz2)      tar xjf $1     ;;
      *.tgz)       tar xzf $1     ;;
      *.zip)       unzip $1       ;;
      *.Z)         uncompress $1  ;;
      *.7z)        7z x $1        ;;
      *)           echo "'$1' cannot be extracted" ;;
    esac
  else
    echo "'$1' is not a valid file"
  fi
}

# Find and kill process by port
killport() {
  kill -9 $(lsof -ti:$1)
}

# Git commit and push
gcp() {
  git add .
  git commit -m "$1"
  git push
}

# Create React component
mkcomp() {
  mkdir -p src/components/$1
  touch src/components/$1/$1.jsx
  touch src/components/$1/$1.css
}

# Backup file with timestamp
backup() {
  cp "$1" "$1.backup-$(date +%Y%m%d-%H%M%S)"
}

# Quick note taking
note() {
  echo "$(date +%Y-%m-%d\ %H:%M:%S): $*" >> ~/notes.txt
}

# Show most used commands
histop() {
  history | awk '{print $2}' | sort | uniq -c | sort -rn | head -20
}
```

### Keyboard Shortcuts (Bash/Zsh)
```bash
# Navigation
Ctrl+A                      # Beginning of line
Ctrl+E                      # End of line
Ctrl+U                      # Delete to beginning of line
Ctrl+K                      # Delete to end of line
Ctrl+W                      # Delete previous word
Alt+B                       # Move backward one word
Alt+F                       # Move forward one word

# Control
Ctrl+C                      # Kill current command
Ctrl+Z                      # Suspend current command
Ctrl+D                      # Exit shell
Ctrl+L                      # Clear screen

# Search
Ctrl+R                      # Search command history
Ctrl+G                      # Exit history search
Ctrl+P                      # Previous command
Ctrl+N                      # Next command
```

### One-Liners
```bash
# Find large files
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null

# Find old log files
find /var/log -name "*.log" -mtime +30

# Count files in directory
find . -type f | wc -l

# Disk usage top 10
du -ah . | sort -rh | head -10

# Check all ports in use
netstat -tulpn | grep LISTEN

# Find process using most memory
ps aux --sort=-%mem | head -n 5

# Find process using most CPU
ps aux --sort=-%cpu | head -n 5

# Monitor file changes
watch -n 2 'ls -lh file.txt'

# Continuous ping with timestamp
ping google.com | while read line; do echo "$(date): $line"; done

# Parallel execution
cat urls.txt | xargs -P 10 -I {} curl -s {}

# JSON pretty print
cat data.json | python -m json.tool
cat data.json | jq '.'

# Generate random password
openssl rand -base64 32

# Check SSL certificate expiry
echo | openssl s_client -servername example.com -connect example.com:443 2>/dev/null | openssl x509 -noout -dates

# Find broken symlinks
find . -type l ! -exec test -e {} \; -print

# Batch rename files
for f in *.txt; do mv "$f" "${f%.txt}.md"; done

# Quick HTTP server
python3 -m http.server 8000
php -S localhost:8000

# Download entire website
wget -r -p -k http://example.com

# Convert line endings (DOS to Unix)
dos2unix file.txt
sed -i 's/\r$//' file.txt

# Remove duplicate lines
sort file.txt | uniq > unique.txt

# Count lines of code
find . -name '*.js' | xargs wc -l

# Find TODO comments in code
grep -rn "TODO" --include="*.js" .

# Check if URL is up
curl -Is http://example.com | head -1

# Monitor log in real-time with color
tail -f app.log | grep --color=always -E 'ERROR|WARNING|
```

---

## Text Processing

### sed (Stream Editor)
```bash
# Replace text
sed 's/old/new/' file.txt
sed 's/old/new/g' file.txt    # All occurrences
sed -i 's/old/new/g' file.txt # In-place edit

# Delete lines
sed '/pattern/d' file.txt
sed '5d' file.txt             # Delete line 5
sed '1,5d' file.txt           # Delete lines 1-5

# Insert/append
sed '3i\New line' file.txt    # Insert before line 3
sed '3a\New line' file.txt    # Append after line 3

# Multiple operations
sed -e 's/old/new/g' -e 's/foo/bar/g' file.txt
```

### awk (Text Processing)
```bash
# Print columns
awk '{print $1}' file.txt     # First column
awk '{print $1,$3}' file.txt  # Columns 1 and 3
awk '{print $NF}' file.txt    # Last column

# Filter
awk '/error/' file.txt        # Lines containing 'error'
awk '$3 > 100' file.txt       # Where column 3 > 100

# Sum column
awk '{sum+=$1} END {print sum}' file.txt

# Count lines
awk 'END {print NR}' file.txt

# Custom delimiter
awk -F',' '{print $1}' file.csv
awk -F':' '{print $1}' /etc/passwd

# Format output
awk '{printf "%-10s %s\n", $1, $2}' file.txt
```

### cut (Extract Columns)
```bash
# By delimiter
cut -d':' -f1 /etc/passwd     # First field
cut -d',' -f1,3 file.csv      # Fields 1 and 3

# By character position
cut -c1-10 file.txt           # Characters 1-10
```

### sort & uniq
```bash
# Sort
sort file.txt
sort -r file.txt              # Reverse
sort -n file.txt              # Numeric sort
sort -u file.txt              # Unique sorted

# Unique lines
uniq file.txt
uniq -c file.txt              # Count occurrences
uniq -d file.txt              # Show duplicates only

# Sort and unique
sort file.txt | uniq
```

### wc (Word Count)
```bash
wc file.txt                   # Lines, words, characters
wc -l file.txt                # Count lines
wc -w file.txt                # Count words
wc -c file.txt                # Count bytes
```

---

## Compression & Archives

### tar
```bash
# Create archive
tar -czf archive.tar.gz directory/
tar -czf backup.tar.gz file1 file2 file3

# Extract
tar -xzf archive.tar.gz
tar -xzf archive.tar.gz -C /destination/

# List contents
tar -tzf archive.tar.gz

# Create without compression
tar -cf archive.tar directory/

# Extract specific file
tar -xzf archive.tar.gz path/to/file
```

### zip/unzip
```bash
# Create zip
zip -r archive.zip directory/
zip archive.zip file1 file2

# Extract
unzip archive.zip
unzip archive.zip -d /destination/

# List contents
unzip -l archive.zip

# Add to existing zip
zip -ur archive.zip newfile.txt
```

### gzip/gunzip
```bash
# Compress
gzip file.txt               # Creates file.txt.gz
gzip -k file.txt            # Keep original

# Decompress
gunzip file.txt.gz
gzip -d file.txt.gz

# View compressed file
zcat file.txt.gz
zless file.txt.gz
```

---

## File Permissions Quick Reference

### Numeric Permissions
```
7 = rwx (read, write, execute)
6 = rw- (read, write)
5 = r-x (read, execute)
4 = r-- (read only)
3 = -wx (write, execute)
2 = -w- (write only)
1 = --x (execute only)
0 = --- (no permissions)
```

### Common Patterns
```bash
chmod 755 script.sh         # rwxr-xr-x
chmod 644 file.txt          # rw-r--r--
chmod 600 secret.txt        # rw-------
chmod 777 shared/           # rwxrwxrwx (avoid!)
chmod 700 private/          # rwx------

# Symbolic
chmod +x script.sh          # Add execute
chmod u+w file.txt          # Add write for owner
chmod go-w file.txt         # Remove write for group/others
chmod a+r file.txt          # Add read for all

# Recursive
chmod -R 755 directory/
```

---

## Cron Jobs (Scheduled Tasks)

### Crontab Syntax
```
* * * * * command
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, Sunday = 0 or 7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

### Crontab Commands
```bash
# Edit crontab
crontab -e

# List crontab
crontab -l

# Remove crontab
crontab -r

# Edit for another user
sudo crontab -u username -e
```

### Examples
```bash
# Run every minute
* * * * * /path/to/script.sh

# Run at 3 AM daily
0 3 * * * /path/to/backup.sh

# Run every hour
0 * * * * /path/to/script.sh

# Run at 9 AM on weekdays
0 9 * * 1-5 /path/to/script.sh

# Run every 5 minutes
*/5 * * * * /path/to/script.sh

# Run on first day of month
0 0 1 * * /path/to/script.sh

# Run every Sunday at midnight
0 0 * * 0 /path/to/script.sh

# Redirect output to log
0 2 * * * /path/to/script.sh >> /var/log/script.log 2>&1

# Run with specific environment
0 3 * * * cd /app && /usr/bin/node script.js

# Multiple commands
0 4 * * * /path/to/backup.sh && /path/to/notify.sh
```

---

## Package Management

### Ubuntu/Debian (apt)
```bash
# Update package list
sudo apt update

# Upgrade packages
sudo apt upgrade
sudo apt full-upgrade

# Install package
sudo apt install nginx
sudo apt install nodejs npm

# Remove package
sudo apt remove nginx
sudo apt purge nginx         # Remove with config files

# Search
apt search nginx
apt-cache search nginx

# Show package info
apt show nginx
apt-cache show nginx

# List installed
apt list --installed
dpkg -l

# Clean up
sudo apt autoremove
sudo apt autoclean
```

### CentOS/RHEL (yum/dnf)
```bash
# Update
sudo yum update
sudo dnf update

# Install
sudo yum install nginx
sudo dnf install nginx

# Remove
sudo yum remove nginx
sudo dnf remove nginx

# Search
yum search nginx
dnf search nginx

# List installed
yum list installed
dnf list installed
```

### Snap
```bash
# Install
sudo snap install node --classic
sudo snap install code --classic

# List installed
snap list

# Update
sudo snap refresh

# Remove
sudo snap remove node
```

---

## Security Basics

### Firewall (UFW)
```bash
# Enable/disable
sudo ufw enable
sudo ufw disable

# Status
sudo ufw status
sudo ufw status verbose

# Allow/deny
sudo ufw allow 22            # SSH
sudo ufw allow 80            # HTTP
sudo ufw allow 443           # HTTPS
sudo ufw allow 3000          # Custom port
sudo ufw deny 23             # Deny telnet

# Delete rule
sudo ufw delete allow 80

# Reset
sudo ufw reset
```

### File Permissions Security
```bash
# Secure SSH keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/authorized_keys

# Secure configuration files
chmod 600 .env
chmod 600 ~/.aws/credentials
chmod 600 ~/.ssh/config

# Check for world-writable files (security risk)
find / -perm -002 -type f 2>/dev/null
```

### User Security
```bash
# Change password
passwd
sudo passwd username

# Lock/unlock user
sudo passwd -l username      # Lock
sudo passwd -u username      # Unlock

# Password expiry
sudo chage -l username       # View expiry
sudo chage -M 90 username    # Max 90 days
```

---

## Quick Troubleshooting Checklist

### Service Won't Start
```bash
1. Check status: sudo systemctl status service_name
2. Check logs: sudo journalctl -u service_name -n 50
3. Check configuration: service_name -t (e.g., nginx -t)
4. Check permissions: ls -la /path/to/files
5. Check disk space: df -h
6. Check memory: free -h
7. Check ports: lsof -i :PORT
```

### Application Errors
```bash
1. Check application logs: tail -f app.log
2. Check environment: printenv | grep NODE
3. Check process: ps aux | grep node
4. Check resources: top or htop
5. Restart application: pm2 restart app
```

### Connection Issues
```bash
1. Ping server: ping example.com
2. Check DNS: nslookup example.com
3. Check port: telnet example.com 80
4. Check firewall: sudo ufw status
5. Check routing: traceroute example.com
6. Test with curl: curl -v http://example.com
```

### Performance Issues
```bash
1. Check CPU: top -o %CPU
2. Check memory: free -h
3. Check disk I/O: iostat
4. Check network: iftop
5. Check logs: grep -i error /var/log/syslog
6. Check processes: ps aux --sort=-%mem
```

---

## Essential Tools to Install

```bash
# Development
sudo apt install git curl wget vim nano

# Build tools
sudo apt install build-essential

# Network tools
sudo apt install net-tools netcat nmap

# Monitoring
sudo apt install htop iotop iftop

# Modern alternatives
sudo apt install ripgrep fd-find bat exa

# Docker
curl -fsSL https://get.docker.com | sh

# Node.js (via nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts

# Docker Compose
sudo apt install docker-compose-plugin
```

---

## Resources & Learning

### Man Pages
```bash
man command_name            # View manual
man -k keyword              # Search manuals
command --help              # Quick help
```

### Online Resources
- **Linux Documentation**: man7.org, linux.die.net
- **Cheat sheets**: cheat.sh (curl cheat.sh/command)
- **Interactive**: explainshell.com
- **Practice**: overthewire.org/wargames

### Quick Tips
```bash
# Command not found? Find which package provides it
apt-file search command_name

# Forgot sudo? Repeat with sudo
sudo !!

# Copy command output to clipboard (requires xclip)
command | xclip -selection clipboard

# Create alias on the fly
alias_name='command'
```

---

## Final Tips for Full-Stack Devs

1. **Learn tmux/screen** for managing multiple terminal sessions
2. **Use dotfiles** to sync your configuration across machines
3. **Master Git** - it's essential for daily work
4. **Automate repetitive tasks** with shell scripts
5. **Use PM2 or similar** for Node.js process management
6. **Learn Docker** for consistent development environments
7. **Set up SSH keys** for passwordless authentication
8. **Use aliases** to speed up common commands
9. **Monitor your resources** before issues occur
10. **Keep logs clean** and rotate them regularly

---

**Remember**: The best way to learn Linux is by using it daily. Start with basic commands, then gradually add more advanced techniques to your workflow. Save this cheat sheet and refer to it often!

**Pro tip**: Create your own cheat sheet with commands you use most frequently. Your workflow is unique to you!
