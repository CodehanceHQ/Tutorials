```
module Types
  class MutationType < Types::BaseObject
    # Other mutations...

    field :createLog, mutation: Mutations::LogMutations::CreateLog
  end
end
```

```
mkdir -p app/graphql/mutations/log_mutations
touch app/graphql/mutations/log_mutations/create_log.rb
```

```
# frozen_string_literal: true

module Mutations
  module LogMutations
    # Create logs for the application
    class CreateLog < Mutations::BaseMutation
      argument :level, String, required: true
      argument :message, String, required: true
      argument :timestamp, String, required: true
      argument :data, GraphQL::Types::JSON, required: false

      field :status, String, null: false
      field :message, String, null: false

      def resolve(level:, message:, timestamp:, data: nil)
        user = context[:current_user] || nil
        log_entry = build_log_entry(level, message, timestamp, data, user)

        if log_entry.save
          success_response
        else
          error_response(log_entry)
        end
      end

      private

      def build_log_entry(level, message, timestamp, context, user)
        LogEntry.new(
          level:,
          message:,
          timestamp:,
          context:,
          user:
        )
      end

      def success_response
        { status: 'success', message: 'Log entry created' }
      end

      def error_response(log_entry)
        { status: 'error', message: log_entry.errors.full_messages.join(', ') }
      end
    end
  end
end
```

```
rails generate model LogEntry level:string message:text timestamp:datetime context:text user:references
```

### edit migration
##### sqlite does not support jsonb but postgres does
```
class CreateLogEntries < ActiveRecord::Migration[7.1]
  def change
    create_table :log_entries do |t|
      t.string :level
      t.text :message
      t.datetime :timestamp
      t.references :user, foreign_key: true, null: true

      if ActiveRecord::Base.connection.adapter_name == 'PostgreSQL'
        t.jsonb :context, default: {}
      else
        t.text :context
      end

      t.timestamps
    end

    if ActiveRecord::Base.connection.adapter_name == 'PostgreSQL'
      add_index :log_entries, :context, using: :gin
    end
  end
end
```

### Add validations to Model
```
class LogEntry < ApplicationRecord
  belongs_to :user, optional: true # optional: true allows log entries to exist without a user
  
  validates :level, presence: true
  validates :message, presence: true
  validates :timestamp, presence: true
end
```

### User Model
```
class User < ApplicationRecord
  has_many :log_entries, dependent: :destroy
  ...
end
```

```
rails db:migrate
```