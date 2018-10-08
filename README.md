### PaperTrail
---

https://github.com/paper-trail-gem/paper_trail

```
bundle exec rails g paper_trail:install [--with-changes] [--with-associations]
bundle exec rake db:migrate
```

```ruby
class Widget < ActiveRecord::Base
  has_paper_trail
end

class ApplicationController
  before_action :set_paper_trail_whodunnit
end

widget = Widget.find 42
widget.versions

v = widget.versions.last
v.event
v.created_at
v.whodunnit
widget = v.reify

widget = Widget.find 153
widget.name

widget = Widget.find 153
widget.name

widget.versions
widget.update_attributes name: 'Wotsit'
widget.versions.last.reify.name
widget.versions.last.event

class Widget < ActiveRecord::Base
  has_paper_trail
end
widget.versions
widget.versions
widget.version
widget.paper_trail.live?
widget.paper_trail.originator
widget.paper_trail.version_at(timestamp)
widget.paper_trail.previous_version
widget.paper_trail.next_version

version.reify(options = {})
version.reify(dup: true)
version.paper_trail_originator
version.terminator
version.whodunnit
version.version_author
version.next
version.previous
version.index
version.event

class Article < ActiveRecord::Base
  has_paper_trail on: [:update]
end

a = Article.create
a.versions.size
a.versions.last.event
a.paper_trail_event = 'update title'
a.update_attributes title: 'My Title'
a.versions.size
a.versions.last.event
a.paper_trail_event = nil
a.update_attributes title: 'Alternate'
a.versions.size
a.versions.last.event

class Article < ActiveRecord::Base
  has_paper_trail on: []
  paper_trail.on_destroy
  paper_trail.on_update
  paper_trail.on_create
  paper_trail.on_touch
end

class Translation < ActiveRecord::Base
  has_paper_trail if: Proc.new { |t| t.language_code == 'US' }
                  unless: Proc.new { |t| t.type == 'DRAFT'}
end

my_model.paper_trail.save_with_version

class Article < ActiveRecord::Base
  has_paper_trail ignore: [:title, :rating]
end

a = Article.create
a.versions.length
a.update_attributes title: 'My Title', raging: 3
a.versions.length
a.update_attributes title: 'Greeting', content: 'Hello'
a.versions.length
a.paper_trail.previous_version.title

class Article < ActiveRecord::Base
  has_paper_trail only: [:title]
end

a = Article.create
a.versions.length
a.update_attributes title: 'My Title'
a.versions.length
a.update_atttributes content: 'Hello'
a.versions.length
a.paper_trail.previous_version.content

class Article < ActiveRecord::Base
  has_paper_trail only: { title: Proc.new { |obj| !obj.title.blank? } }
end

a = Article.create
a.versions.length
a.update_attributes content: 'Hello'
a.update_attributes title: 'Title One'
a.versions.length
a.update_attributes content: 'Hai'
a.versions.length
a.paper_trail.previous_version.content
a.update_attributes title: 'Title Two'
a.versions.length
a.paper_trail.previous_version.content

class Article < ActiveRecord::Base
  has_paper_trail skip: [:file_upload]
end

PaperTrail.enalbed = false

# config/environments/test.rb
config.paper_trail.enabled = false

PaperTrail.request(enabled: false) do
end

PaperTrail.request.enabled = false
PaperTrail.request.enabled = true

PaperTrail.request.disable_model(Banana)
PeperTrail.request.enable_model(Banana)
PaperTrail.request.enabled_for_model?(Banana)

class ApplicationController < ActionController::Base
  def paper_trail_enabled_for_controller
    super && request.user_agent != 'Disable User-Agent'
  end
end

PaperTrail.config.version_limit = 3
PaperTrail.config.version_limit = nil

widget = Widget.find 42
widget.update_attributes name: 'tky tky'
widget = widget.paper_trail.previous_version
widget.save

widget = widget.paper_trail.version_at(1.day.ago)
widget.save

widget = Widget.find(42)
widget.destroy
versions = widget.versions
widget = versions.last.reify
widget.save

live_widget = Widget.find 42
live_widget.versions.length
widget = live_widget.paper_trail.previous_version
widget = widget.paper_trail.previous_version
widget = widget.paper_trail.next_version
widget.paper_trail.next_version

widget = Widget.find 42
version = widget.versinon[-2]
previous = version.previous
next = version.next

current_version_number = version.index

latest_version = Widget.find(42).version.last
widget = latest_version.reify
widget.version == latest_version

widget = Widget.find 42
widget.live?
widget = widget.paper_trail.perevious_version
widget.live?

PaperTrail::Version.where_object(content: 'Hello', title: 'Article')
PaperTrail::Version.where_object_changes(atr: 'v')

widget = Widget.create name: 'tky'
widget.versions.last.changeset
# => {
# =>  "name" =>[nil, "tky"],
# =>  "created_at" => [nil, 2018-10-08 10:00:10 UTC]
# =>  "updated_at" =>[nil, 2018-10-08 10:00:10 UTC],
# =>  "id" => [nil, 1]
# => }
widget.update_attributes name: 'tky'
widget.versions.last.changeset
# => {
# =>  "name" =>["tky", "tky"],
# =>  "updated_at" => [2018-10-08 10:00:10 UTC, 2018-10-08 10:00:10 UTC]
# => }
widget.destroy
widget.versions.last.changeset
# => {}

sql> delete from versions where created_at < 2018-10-01;
PaperTrail::Version.delete_all ['created_at < ?', 1.week.ago]

PaperTrail.request.whodunnit = 'tky tky'
widget.update_attributes name: 'tky'
widget.versions.last.whodunnit

PaperTrail.request.whodunnit = proc do
  caller.find { |c| c.starts_with? Rails.root.to_s }
end















```

