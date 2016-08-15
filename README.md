# rails-db-proxy

A better approach for using a primary and secondary database setup in
Rails.

### Design

The idea is that this library would manage the connections based on the
existing database YAML file with minimum changes and remain compatible
with the base ActiveRecord implementation. Here is the proposed changes
to the `database.yml` file.

```yaml
production:
  adapter: mysql2
  username: user
  password: super_s3cret
  host: my-db-host.domain.internal

  connections:
    primary: my-db-host.domain.internal
    secondary: my-secondary-db-host.domain.internal
```

### Usage examples

Model chaining

```rb
User.find(1).using_database_connection(:secondary)
```

For the whole model

```rb
class User < ActiveRecord::Base
  use_database_connection :secondary
end
```

...Or individual methods

```rb
# For only a certain methods
class User < ActiveRecord::Base
  use_database_connection :secondary, :only => [:get_all_users]
end

# For all methods in a model except for a subset of them
class User < ActiveRecord::Base
  use_database_connection :secondary, :except => [:quick_method]
end
```
