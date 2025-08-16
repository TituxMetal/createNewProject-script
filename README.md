# createNewProject

A sophisticated bash script that automates the creation of full-stack monorepo projects by cloning
and customizing a sample project template.

## ✨ Features

- 🚀 **Automated Project Setup** - Creates complete monorepo projects with one command
- 📅 **Date-based Naming** - Automatically generates project names in `YYYYMMDD-project-name` format
- 🐙 **GitHub Integration** - Creates GitHub repositories and handles existing repos gracefully
- 🔄 **Smart Reference Replacement** - Updates all project-specific references throughout the
  codebase
- 📦 **Node.js & Yarn Management** - Automatically installs latest LTS Node.js via nvm and
  configures Yarn via corepack
- 🗃️ **Database Setup** - Initializes Prisma client and database schema
- 📁 **Environment Configuration** - Creates default .env files for both API and web applications
- 🖥️ **IDE Integration** - Opens the project in Cursor IDE automatically
- 🔍 **Verbose Logging** - Clear progress indication with emojis and detailed status messages

## 📋 Prerequisites

Before using this script, ensure you have the following installed:

- **git** - Version control
- **gh** (GitHub CLI) - For GitHub repository management (must be authenticated)
- **nvm** - Node Version Manager for Node.js installation
- **yarn** - Package manager (will be configured via corepack)
- **cursor** - IDE for opening the project
- **curl** - For fetching latest version information
- **jq** - JSON processor for parsing API responses

## 🏗️ Directory Structure

The script expects and uses the following directory structure:

```treeview
~/webdev/
├── labo/                    # Default location for new projects
├── projects/               # Alternative location (with --projects flag)
└── labo/sample-project/    # Source template project (required)
```

## 🚀 Installation

### Method 1: Self-Installation (Recommended)

```bash
# Clone this repository
git clone <this-repo-url>
cd createNewProject-script

# Make the script executable
chmod +x createNewProject

# Install the script to ~/bin/
./createNewProject --install
```

### Method 2: Manual Installation

```bash
# Copy to ~/bin directory
cp createNewProject ~/bin/createNewProject
chmod +x ~/bin/createNewProject

# Add alias to your shell configuration
echo 'alias createNewProject="~/bin/createNewProject"' >> ~/.bashrc
source ~/.bashrc
```

## 📖 Usage

### Basic Usage

```bash
# Create a project in ~/webdev/labo/
createNewProject my-awesome-app

# Create a project in ~/webdev/projects/
createNewProject my-awesome-app --projects

# Show help
createNewProject --help
```

### Project Naming

The script automatically creates date-based project names:

- Input: `my-awesome-app`
- Output: `20250816-my-awesome-app` (current date + project name)

### Command Options

| Option         | Description                                                        |
| -------------- | ------------------------------------------------------------------ |
| `project-name` | Name of the project (required, alphanumeric and hyphens only)      |
| `--projects`   | Create project in `~/webdev/projects/` instead of `~/webdev/labo/` |
| `--install`    | Install the script to `~/bin/createNewProject`                     |
| `--help`, `-h` | Show usage information                                             |

## 🔧 What the Script Does

1. **🔍 Validation** - Checks all dependencies and authentication
2. **📅 Project Setup** - Creates date-based project name and validates paths
3. **🌐 Version Detection** - Fetches latest Node.js LTS and Yarn versions
4. **📦 Repository Cloning** - Clones the sample project template
5. **🐙 GitHub Integration** - Creates/configures GitHub repository
6. **🔄 Reference Replacement** - Updates all project-specific content:
   - Root package.json and README.md
   - Docker configurations and build scripts
   - GitHub workflow files
   - Apps package.json files (API and Web)
   - Environment example files
7. **📦 Node.js Setup** - Installs and configures latest LTS Node.js
8. **🧶 Yarn Configuration** - Sets up Yarn via corepack
9. **📦 Dependency Installation** - Installs all project dependencies
10. **📄 Environment Files** - Creates default .env files
11. **🗃️ Database Setup** - Initializes Prisma client and database
12. **🖥️ IDE Launch** - Opens project in Cursor

## 📁 Generated Project Structure

After successful execution, your project will contain:

```treeview
20250816-project-name/
├── apps/
│   ├── api/
│   │   ├── .env                    # Created with DATABASE_URL
│   │   └── package.json           # Updated with project name
│   └── web/
│       ├── .env                    # Created with API URLs
│       └── package.json           # Updated with project name
├── .nvmrc                          # Latest Node.js LTS version
├── .yarnrc.yml                     # Copied from sample project
├── README.md                       # Updated with project name
├── package.json                    # Updated with project name
└── .git/                           # New repository linked to GitHub
```

## 🔄 Replacement Patterns

The script replaces the following patterns throughout the codebase:

| Pattern                | Replaced With         | Usage                      |
| ---------------------- | --------------------- | -------------------------- |
| `sample-project`       | `your-project-name`   | General project references |
| `{project-name}`       | `your-project-name`   | Template placeholders      |
| `{your-username}`      | `TituxMetal`          | GitHub username            |
| `{your-dockerhub-org}` | `lgdweb`              | Docker Hub organization    |
| `# Sample Project`     | `# your-project-name` | README titles              |

## 🐙 GitHub Integration

The script intelligently handles GitHub repositories:

- **New Repository**: Creates a private repository and configures remote
- **Existing Repository**: Detects existing repos and configures remote without failing
- **Authentication**: Requires `gh auth login` to be completed beforehand

## 🗃️ Database Configuration

### Default Environment Variables

**API (.env)**:

```env
DATABASE_URL="file:./dev.db"
```

**Web (.env)**:

```env
API_URL=http://localhost:3000
PUBLIC_API_URL=/
```

### Prisma Setup

The script automatically runs:

```bash
yarn workspace @app/api prisma generate
yarn workspace @app/api prisma db push
```

## 🛠️ Architecture

### Code Structure

- **No if/else constructs** - Uses case statements and logical operators
- **camelCase functions** - All function names follow camelCase convention
- **Modular design** - Each function has a single responsibility
- **Consistent logging** - Unified logging system with emojis and proper error handling

### Error Handling

- **Early validation** - Checks dependencies and prerequisites upfront
- **Graceful failures** - Clear error messages with actionable guidance
- **Rollback safety** - Fails fast to prevent partial project states

## 📊 Logging System

The script provides detailed progress information:

| Icon | Type     | Description            |
| ---- | -------- | ---------------------- |
| 🔍   | Check    | Validation steps       |
| 🔄   | Progress | Work in progress       |
| ✅   | Success  | Completed successfully |
| ❌   | Error    | Error messages         |
| ⚠️   | Warning  | Warning messages       |
| 🚀   | Step     | Major milestones       |

## 🐛 Troubleshooting

### Common Issues

**Dependencies not found**:

```bash
# Install missing dependencies
brew install gh nvm yarn curl jq          # macOS
sudo apt install gh curl jq               # Debian (nvm: https://github.com/nvm-sh/nvm#installing-and-updating)
sudo pacman -S github-cli curl jq nvm     # Arch Linux
```

**GitHub authentication**:

```bash
gh auth login
```

**Sample project missing**:

```bash
# Ensure sample project exists at:
ls ~/webdev/labo/sample-project
```

**Yarn command not found**:

- The script handles this automatically via corepack
- Ensure Node.js is properly installed via nvm

### Debug Mode

For troubleshooting, run with bash debug mode:

```bash
bash -x createNewProject project-name
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Follow the existing code style (no if/else, camelCase functions, 2-space indentation)
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 🔗 Related Projects

- [Sample Project Template](https://github.com/TituxMetal/sample-project) - The source template
- [GitHub CLI](https://cli.github.com/) - GitHub command line tool
- [Node Version Manager](https://github.com/nvm-sh/nvm) - Node.js version management

---

**Created with ❤️ for streamlined full-stack development**
