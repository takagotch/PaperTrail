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

a




















```

