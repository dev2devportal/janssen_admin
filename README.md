# janssen_admin
Hawke's Janssen Identity &amp; Access Management Server WebUI Administration Tools

Hawke's D2D Janssen IAM Web Admin Interface

A comprehensive web-based administration interface for managing Janssen IAM (Identity and Access Management) servers. This application provides an intuitive GUI for managing users, groups, roles, and OAuth scopes without needing to use the command line or API directly.
Features
Core Management Features

    User Management: Create, read, update, and delete users via SCIM API
    Group Management: Manage user groups and memberships
    OAuth Client Management:
        Comprehensive client creation with 40+ configuration fields
        Full client editing capabilities with tabbed interface
        View and copy client credentials securely
        Regenerate client secrets on demand
        Support for all OAuth 2.0/OIDC grant types and flows
    OAuth Scope Management: Configure and manage OAuth 2.0 scopes
    Statistics Dashboard: Real-time counts of users, groups, clients, and scopes

Security Features

    Secure Authentication: Direct integration with Janssen authentication
    Session Management: Secure session handling with automatic token refresh
    Client Secret Management:
        Secure viewing with show/hide toggle
        Copy to clipboard functionality
        Automatic secret generation (40 characters)
        Support for multiple authentication methods
    CSRF Protection: Built-in CSRF protection for all forms
    Security Headers: Comprehensive security headers implementation

User Interface

    Responsive UI: Bootstrap 5-based interface that works on desktop and mobile
    Tabbed Forms: Organized configuration with Basic, Authentication, Tokens, and Advanced tabs
    Real-time Updates: Dynamic loading of available scopes and configurations
    Copy to Clipboard: Quick copy functionality for IDs and secrets
    Loading Indicators: Visual feedback during operations
    Error Handling: User-friendly error messages with detailed logging

Architecture

    Backend: Python 3.11+ with Flask framework
    Authentication: OAuth 2.0 / OpenID Connect with Janssen server
    APIs:
        Janssen Config API for OAuth client and scope management
        SCIM API for user and group management
        Service account authentication for API access
    Database: SQLite for session and user storage
    Frontend:
        Bootstrap 5 for responsive design
        Vanilla JavaScript for dynamic interactions
        Bootstrap Icons for UI elements
    Security:
        HTTPS support with configurable certificates
        Comprehensive security headers
        CSRF protection on all forms
        Secure token storage and refresh

Prerequisites

    Python 3.8 or higher
    Access to a Janssen IAM server
    Admin credentials for the Janssen server
    Linux/Unix environment (tested on Debian 12)

Installation
1. Clone the Repository

git clone https://git.dev2dev.net/Dev2Dev/d2d_iam_webui.git
cd d2d_iam_webui

2. Set Up Python Virtual Environment

python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

3. Configure Environment Variables

Copy the environment template and edit with your settings:

cp .env.template .env
nano .env

Key configuration variables:

# Janssen Server Configuration
JANSSEN_SERVER_URL=https://your-janssen-server.com
JANSSEN_ADMIN_USER=admin@example.com
JANSSEN_ADMIN_PASSWORD=your-admin-password

# Flask Configuration
SECRET_KEY=generate-a-secure-random-key
FLASK_ENV=development  # or production

# Optional OAuth Configuration
OIDC_CLIENT_ID=your-client-id
OIDC_CLIENT_SECRET=your-client-secret

4. Initialize the Database

flask init-db

Usage
Starting the Application
Development Mode

./scripts/start.sh

Or manually:

source venv/bin/activate
python app.py

The application will be available at: http://localhost:5000
Production Mode

For production, use the provided script with Gunicorn:

export FLASK_ENV=production
./scripts/start.sh

Management Scripts

The application includes management scripts in the scripts/ directory:

    start.sh: Start the application
    stop.sh: Stop the application
    restart.sh: Restart the application
    status.sh: Check application status

Using the Web Interface

    Login: Navigate to http://localhost:5000 and log in with your Janssen admin credentials

    Dashboard: View statistics and quick actions for user/group/scope management

    User Management:
        View all users with pagination
        Search and filter users
        Create new users
        Edit user details
        Delete users (admin only)

    Group Management:
        View and manage groups
        Add/remove group members
        Create new groups

    Scope Management:
        View OAuth scopes
        Create custom scopes
        Edit scope permissions

API Endpoints

The application exposes RESTful API endpoints:
Authentication

    POST /auth/login - User login
    GET /auth/logout - User logout
    GET /auth/profile - View user profile

User Management

    GET /api/users - List users
    GET /api/users/<id> - Get user details
    POST /api/users - Create user
    PUT /api/users/<id> - Update user
    DELETE /api/users/<id> - Delete user

Group Management

    GET /api/groups - List groups
    GET /api/groups/<id> - Get group details
    POST /api/groups - Create group
    PUT /api/groups/<id> - Update group
    DELETE /api/groups/<id> - Delete group

Scope Management

    GET /api/scopes - List scopes
    GET /api/scopes/<inum> - Get scope details
    POST /api/scopes - Create scope
    PUT /api/scopes/<inum> - Update scope
    DELETE /api/scopes/<inum> - Delete scope

Configuration
Environment Variables

See .env.template for all available configuration options.
Feature Flags

Enable/disable features via environment variables:

ENABLE_USER_MANAGEMENT=True
ENABLE_GROUP_MANAGEMENT=True
ENABLE_ROLE_MANAGEMENT=True
ENABLE_SCOPE_MANAGEMENT=True
ENABLE_CLIENT_MANAGEMENT=True
ENABLE_SCRIPT_MANAGEMENT=True

Logging

Logs are stored in the logs/ directory:

    app.log - Main application log
    access.log - HTTP access log (production)
    error.log - Error log (production)

Configure logging level via LOG_LEVEL environment variable.
Security Considerations

    Always use HTTPS in production
    Keep SECRET_KEY secure and random
    Use strong admin passwords
    Regularly update dependencies
    Enable security headers (automatically configured)
    Implement rate limiting for production deployments
    Use a reverse proxy (nginx/Apache) in production

Testing
Run Tests

pytest tests/

Test Coverage

pytest --cov=app tests/

Manual Testing

Test the Janssen connection:

flask test-janssen-connection

Troubleshooting
Common Issues

    Connection to Janssen Failed
        Verify JANSSEN_SERVER_URL is correct
        Check network connectivity
        Ensure SSL certificates are valid

    Authentication Failed
        Verify admin credentials
        Check if user has required permissions
        Review logs/app.log for details

    Database Errors
        Run flask init-db to initialize
        Check file permissions on database file
        Review DATABASE_URL configuration

Debug Mode

Enable debug mode for detailed error messages:

export FLASK_ENV=development
export FLASK_DEBUG=True
python app.py

Development
Project Structure

d2d_iam_janssen_admin_webui/
├── app/
│   ├── __init__.py         # Application factory
│   ├── api/                # API routes
│   ├── auth/               # Authentication modules
│   ├── models/             # Database models
│   ├── templates/          # HTML templates
│   └── static/             # CSS, JS, images
├── config/
│   └── config.py           # Configuration classes
├── scripts/                # Management scripts
├── logs/                   # Application logs
├── tests/                  # Test suite
├── requirements.txt        # Python dependencies
├── .env.template           # Environment template
└── app.py                  # Application entry point

Contributing

    Fork the repository
    Create a feature branch
    Make your changes
    Add tests for new functionality
    Ensure all tests pass
    Submit a pull request

Code Style

    Follow PEP 8 guidelines
    Use type hints where appropriate
    Add docstrings to all functions/classes
    Keep functions focused and small
    Write comprehensive error messages

Future Enhancements

    Role-based access control (RBAC)
    Audit logging
    Bulk operations
    Import/export functionality
    Advanced search and filtering
    Real-time notifications
    Multi-language support
    Dark mode theme
    API rate limiting
    Two-factor authentication


Support

For issues, questions, or contributions:

    Create an issue in the Git repository
    Contact the Dev 2 Dev Portal LLC Support Team

Acknowledgments

    Hawke Robinson for creating this much-needed tool (https://www.hawkerobinson.com)
    Janssen Project for the excellent IAM platform
    Flask community for the web framework
    Bootstrap team for the UI components

Version: 2025.08.090124 Last Updated: August 2025
Maintained By: Dev 2 Dev Portal LLC's Founder Hawke Robinson
