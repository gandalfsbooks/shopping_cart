[![Overall](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fapp.opslevel.com%2Fapi%2Fservice_level%2FAILxMhrLALfBwq6SWi7NWx5X_WQlQJduI_NjdU4lP9I)](https://app.opslevel.com/services/shopping_cart_service/maturity-report)  
[![Service Ownership](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fapp.opslevel.com%2Fapi%2Fservice_level%2FAILxMhrLALfBwq6SWi7NWx5X_WQlQJduI_NjdU4lP9I%2Fservice_ownership)](https://app.opslevel.com/services/shopping_cart_service/maturity-report)  
[![Security](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fapp.opslevel.com%2Fapi%2Fservice_level%2FAILxMhrLALfBwq6SWi7NWx5X_WQlQJduI_NjdU4lP9I%2Fsecurity_2)](https://app.opslevel.com/services/shopping_cart_service/maturity-report)  
[![Reliability](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fapp.opslevel.com%2Fapi%2Fservice_level%2FAILxMhrLALfBwq6SWi7NWx5X_WQlQJduI_NjdU4lP9I%2Freliability_2)](https://app.opslevel.com/services/shopping_cart_service/maturity-report)  
[![Scalability](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fapp.opslevel.com%2Fapi%2Fservice_level%2FAILxMhrLALfBwq6SWi7NWx5X_WQlQJduI_NjdU4lP9I%2Fscalability_2)](https://app.opslevel.com/services/shopping_cart_service/maturity-report)  
[![Observability](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fapp.opslevel.com%2Fapi%2Fservice_level%2FAILxMhrLALfBwq6SWi7NWx5X_WQlQJduI_NjdU4lP9I%2Fobservability_2)](https://app.opslevel.com/services/shopping_cart_service/maturity-report)  
[![Quality](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fapp.opslevel.com%2Fapi%2Fservice_level%2FAILxMhrLALfBwq6SWi7NWx5X_WQlQJduI_NjdU4lP9I%2Fquality_2)](https://app.opslevel.com/services/shopping_cart_service/maturity-report)  

# Ruby on Rails Tutorial: sample application

**This repository is not current or maintained. See [www.railstutorial.org/help](https://www.railstutorial.org/help) for the current version of the sample app.**

This is the sample application for
[*Ruby on Rails Tutorial: Learn Web Development with Rails*](http://railstutorial.org/)
by [Michael Hartl](http://michaelhartl.com/). You can use this reference implementation to help track down errors if you end up having trouble with code in the tutorial. In particular, as a first debugging check I suggest getting the test suite to pass on your local machine:

    cd /tmp
    git clone https://github.com/railstutorial/sample_app_rails_4.git
    cd sample_app_rails_4
    cp config/database.yml.example config/database.yml
    bundle install --without production
    bundle exec rake db:migrate
    bundle exec rake db:test:prepare
    bundle exec rspec spec/

If the tests don't pass, it means there may be something wrong with your system. If they do pass, then you can debug your code by comparing it with the reference implementation.

## Dependencies

Requires Ruby.

## Backups

Ensure data is replicated across 2 active AWS Availability Zones.

## Disaster Recovery

Test DR plan to GCP every quarter.
