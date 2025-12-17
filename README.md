# Ruby on Rails Development Guide

This guide covers development practices, git workflow, testing, and local setup for our Rails application.

## Development Practices

### Code Style

Follow the [Ruby Style Guide](https://rubystyle.guide/) and use RuboCop for linting. Run `bundle exec rubocop` before committing to catch style violations. Configure your editor to format on save when possible.

### Architecture Guidelines

Keep controllers thin by moving business logic into service objects or model methods. Use concerns sparingly and prefer composition over inheritance. Follow RESTful conventions for routing and controller actions.

```ruby
# Good: Thin controller delegating to a service
class OrdersController < ApplicationController
  def create
    result = Orders::CreateService.call(order_params, current_user)
    
    if result.success?
      redirect_to result.order, notice: "Order placed successfully"
    else
      @order = result.order
      render :new, status: :unprocessable_entity
    end
  end
end
```

### Database Conventions

Write reversible migrations whenever possible. Add database indexes for foreign keys and commonly queried columns. Use `references` with `foreign_key: true` to enforce referential integrity at the database level.

## Git Workflow

### Branch Naming

Use descriptive branch names with a prefix indicating the type of change:

- `feature/add-user-authentication`
- `fix/resolve-cart-total-calculation`
- `chore/upgrade-rails-version`
- `docs/update-api-documentation`

### Commit Messages

Write clear, concise commit messages. Use the imperative mood and keep the subject line under 50 characters. Add a body for complex changes.

```
Add email validation to User model

- Implement custom validator for corporate email domains
- Add error messages for invalid formats
- Update user registration form with validation feedback
```

### Pull Request Process

1. Create a feature branch from `main`
2. Make your changes with atomic, well-described commits
3. Ensure all tests pass locally: `bundle exec rspec`
4. Run the linter: `bundle exec rubocop -A`
5. Push your branch and open a pull request
6. Request review from at least one team member
7. Address feedback and squash commits if needed
8. Merge after approval (prefer squash merge for clean history)

### Code Review Guidelines

When reviewing, check for correctness, test coverage, security implications, and performance. Be constructive and specific in feedback. Approve only when you're confident the code is ready for production.

## Testing

### Test Structure

We use RSpec for testing. Organize tests to mirror the application structure:

```
spec/
├── models/
├── controllers/
├── services/
├── requests/
├── system/
├── support/
└── factories/
```

### Writing Tests

Write tests that are readable, isolated, and reliable. Use factories (FactoryBot) instead of fixtures for test data. Each test should verify one behavior.

```ruby
# spec/models/user_spec.rb
RSpec.describe User, type: :model do
  describe "validations" do
    it "requires an email" do
      user = build(:user, email: nil)
      expect(user).not_to be_valid
      expect(user.errors[:email]).to include("can't be blank")
    end

    it "requires a unique email" do
      create(:user, email: "test@example.com")
      duplicate = build(:user, email: "test@example.com")
      expect(duplicate).not_to be_valid
    end
  end

  describe "#full_name" do
    it "combines first and last name" do
      user = build(:user, first_name: "Jane", last_name: "Doe")
      expect(user.full_name).to eq("Jane Doe")
    end
  end
end
```

### Running Tests

```bash
# Run the full test suite
bundle exec rspec

# Run a specific file
bundle exec rspec spec/models/user_spec.rb

# Run a specific test by line number
bundle exec rspec spec/models/user_spec.rb:15

# Run with coverage report
COVERAGE=true bundle exec rspec
```

### Test Coverage

Aim for meaningful coverage rather than a specific percentage. Critical paths like authentication, payments, and data mutations should have thorough test coverage. Use SimpleCov to track coverage metrics.

## Local Development Setup

### Prerequisites

- Ruby 3.2+ (use rbenv or asdf for version management)
- PostgreSQL 14+
- Node.js 18+ and Yarn
- Redis (for ActionCable and Sidekiq)

### Initial Setup

Clone the repository and install dependencies:

```bash
git clone git@github.com:your-org/your-app.git
cd your-app

# Install Ruby dependencies
bundle install

# Install JavaScript dependencies
yarn install

# Set up environment variables
cp .env.example .env
# Edit .env with your local configuration

# Create and set up the database
bin/rails db:setup
```

### Running the Application

Start all services with a single command using Foreman:

```bash
bin/dev
```

Or start services individually:

```bash
# Rails server
bin/rails server

# Webpack dev server (for asset compilation)
bin/webpack-dev-server

# Sidekiq (background jobs)
bundle exec sidekiq

# Redis (if not running as a system service)
redis-server
```

The application will be available at `http://localhost:3000`.

### Useful Commands

```bash
# Open Rails console
bin/rails console

# Run database migrations
bin/rails db:migrate

# Reset database (drops, creates, migrates, seeds)
bin/rails db:reset

# Generate a new model
bin/rails generate model Article title:string body:text

# View all routes
bin/rails routes
```

### Troubleshooting

If you encounter issues, try these steps:

1. Ensure all services are running (PostgreSQL, Redis)
2. Verify your Ruby version matches `.ruby-version`
3. Run `bundle install` and `yarn install` to ensure dependencies are current
4. Reset the database: `bin/rails db:reset`
5. Clear cached assets: `bin/rails assets:clobber`

For additional help, check the project wiki or reach out in the team Slack channel.
