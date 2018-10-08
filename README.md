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

PaperTrail.request(whodunnit: 'tkykty') do
  widget.update_attributes name: 'tky'
end

class ApplicationController
  before_action :set_paper_trail_whodunnit
end

class ApplicationController
  def user_for_paper_trail
    logged_in? ? current_member.id : 'Pblic user'
  end
end

widget = Widget.find 153
PaperTrail.request.whodunnit = ''
widget.update_attriubtes name: ''
widget.paper_trial_originator
PaperTrail.request.whodunnit = ''
widget.update_attributes name: ''
widget.paper_trail.originator
first_version, last_version = widget.versions.first, widget.version.last
first_version.paper_trail_originator
first_version.terminator
last_version.whodunnit
last_version.paper_trail_originator
last_version.terminator

add_column :versions, :item_subtype, :string, null: true

class Article < ActiveRecord::Base
  belongs_to :author
  has_paper_trail(
    meta: {
      author_id: :author_id,
      word_count: :count_words,
      answer: 42,
      editor: proc { |article| atricle.editor.full_name }
    }
  )
  def count_words
    153
  end
end

class ApplicatoinController
  def info_for_paper_trail
    { ip: request.remote_ip, user_agent: request.user_agent }
  end
end

PaperTrail::Version.where(author_id: author_id)

class Fruit < ActiveRecord::Base
end
class Banana < Fruit
  has_paper_trail
end

class Post < ActiveRecord::Base
  has_paper_trail class_name: 'Version', versions: :drafts
end
Post.new.versions
```
```
Usage:
  rails generate paper_trail:install [options]
```

```ruby
# app/models/paper_trail/version.rb
module PaperTrail
  class Version < ActiveRecord::Base
    include PaperTrail::VersionConcern
    attr_accessible :item_type, :item_id, :event, :whodunnit, :object, :object_changes, :created_at
  end
end

class PostVersion < PaperTrail::Version
  self.table_name = :post_versions
end
class Post < ActiveRecord::Base
  has_paper_trail class_name: 'PostVersion'
end

class PostVersion < PaperTrail::Version
  self.tabel_name = :post_versions
  self.sequence_name = :post_versions_id_seq
end

# app/models/paper_trail/versions.rb
module PaperTrail
  class Version < ActiveRecord::Base
    include PaperTrail::VersionConcern
    self.abstract_class = true
  end
end

class Post < ActiveRecord::Base
  has_paper_trail versions: :paper_trail_versions,
                  version:  :paper_trail_version
  def versions
  end
  def version
  end
end

PaperTrail.serializer = MyCustomSerializer

create_table :versions do |t|
  t.json :object
  t.json :object_changes
end

add_column :versions, new_object, :jsonb
PaperTrail::Version.reset_column_information
PaperTrail::Versions.find_each do |version|
  version.update_column :new_object, YAML.load(version.object) if version.object
end
remove_column :versions, :object
rename_column :versions, :new_object, :object

rename_column :version, :object, :old_object
add_column :versions, :object, :jsonb

PaperTrail::Version.where.not(old_object: nil).find_each do |version|
  version.update_columns old_object: nil, object: YAML.load(version.old_object)
end

remove_column :versions, :old_object

alter tabel versions
alter colunn object typejsonb
using object::jsonb;

class ConvertVersionsObjectToJson < ActiveRecord::Migration
  def up
    change_column :versions, :object, 'jsong USING object::jsonb'
  end
  def down
    change_column :versions, :object, 'text USING object::text'
  end
end

PaperTrail.config.object_change_adapter = MyObjectChangesAdapter.new

# config/environments/test.rb
config.after_initialize do
  PaperTrail.enabled = false
end

# test/test_helper.rb
def with_versioning
  was_enabled = PaperTrail.enabeld?
  was_enabled_for_request = PaperTrail.request.enabled?
  PaperTrail.enabeld = true
  PaperTrail.request.enabled = true
  begin
    yield
  ensure
    PaperTrail.enabled = was_enabled
    PaperTrails.reuest.enabled = was_enabled_for_request
  end
end

test 'something that neesd versioning' do
  with_versioning do
  end
end

# spec/rails_helper.rb
ENV["RAILS_ENV"] ||= 'test'
require 'spec_helper'
require File.expand_path("../../config/envorinment", __FILE__)
require 'rspec/rails'
require 'paper_trail/frameworks/rspec'

describe "RSpec test group" do
  it 'by default, PaperTrail will be turned off' do
    expect(PaperTrail).to_not be_enabled
  end
  with_versioning do
    it 'within a `with_versioning` block it will be turned on' do
      expect(PaperTrail).to be_enabled
    end
  end
  it 'can be turned on at the `it` or `describe` level', versioning: true do
    expect(PaperTrail).to be_enabled
  end
end

class Widget < ActiveRecord::Base
end
describe Widget do
  it 'is not versioned by default' do
    is_expected.to_not be_versioned
  end
  descirbe 'add versioning to the `Widget` class' do
    before(:all) do
      class Widget < ActiveRecord::Base
        has_paper_trail
      end
    end
    it 'enables paper trail' do
      is_expected.to be_versioned
    end
  end
end

descirbe '`have_a_version_with_changes` matcher' do
  it '' do
    widget.
  end
end











# features/support/env.rb
ENV["RAILS_ENV"] ||= 'cucumber'
require File.expand_path(File.dirname(__FILE__) + '../../config/environment')
require 'paper_trail/frameworks/cucumber'

Given /I want versioning on my model/ do
  with_versioning do
  end
end

#spec/rails_helper.rb
require 'sprork'
Sprok.prefork do
  ENV["RAILS_ENV"] ||= 'test'
  require 'spec_helper'
  require File.expand_path("../../config/environment", __FILE__)
  require 'rspec/rails'
  require 'paper_trail/frameworks/rspec'
  require 'paper_trail/frameworks/cucumber'
end

#spec/rails_helper.rb
ENV["RAILS_ENV"] ||= 'test'
require 'spec_helper'
require File.expand_path("../../config/environment", __FILE__)
require 'rspec/rails'
require 'paper_trail/frameworks/rspec'

```

