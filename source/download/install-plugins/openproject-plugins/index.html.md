# OpenProject Plugins

## Contribute Plugins

We encourage you to extend OpenProject yourself by writing a plugin. Please, read the [plugin-contributions](https://community.openproject.org/projects/openproject/wiki/Developing_Plugins) guide for more information.

If you are a plugin author and want your plugins to be listed here, leave us a note in our [forums](https://www.openproject.org/projects/openproject/boards), via [twitter](https://twitter.com/openproject)or any other communication channel. We’re looking forward to your contribution.

Please note that the plugin author is responsible to keep his plugin up to date with OpenProject development. In case a plugin causes errors, please contact the plugin author and/or write in our support forums. We will remove plugins from this page if we notice that they are unmaintained.

## Supported Plugins

Supported plugins are compatible with the current OpenProject version.

### Backlogs

- Description:&nbsp;Features for agile teams
- Author:&nbsp; [Finnlabs](http://www.finn.de/)
- Link: [Source](https://github.com/finnlabs/openproject-backlogs)
- Dependency:&nbsp;PDF Export

```ruby
  gem "openproject-backlogs", :git => "https://github.com/finnlabs/openproject-backlogs.git", :branch => 'stable'
```

### Cost

- Description:&nbsp;Features for planning and tracking project costs
- Author:&nbsp; [Finnlabs](http://www.finn.de/)
- Link: [Source](https://github.com/finnlabs/openproject-costs)

```ruby
  gem 'openproject-costs', :git => 'https://github.com/finnlabs/openproject-costs.git', :branch => 'stable'
```

### Global Roles

- Description:&nbsp;Allows users to create top-level projects
- Author:&nbsp; [Finnlabs](http://www.finn.de/)
- Link: [Source](https://github.com/finnlabs/openproject-global_roles)
- Dependency:

```ruby
  gem 'openproject-global_roles', :git => 'https://github.com/finnlabs/openproject-global_roles.git', :branch => 'stable'
```

### Help Link

- Description:&nbsp;Allows to provide an alternative help link
- Author:&nbsp; [Finnlabs](http://www.finn.de/)
- Link: [Source](https://github.com/finnlabs/openproject-help_link)

```ruby
  gem 'openproject-help_link', :git => 'https://github.com/finnlabs/openproject-help_link.git', :branch => 'stable'
```

### Meeting

- Description:&nbsp;Features to schedule, prepare, and hold meetings
- Author:&nbsp; [Finnlabs](http://www.finn.de/)
- Link: [Source](https://github.com/finnlabs/openproject-meeting)

```ruby
  gem 'openproject-meeting', :git => 'https://github.com/finnlabs/openproject-meeting.git', :branch => 'stable'
```

### My Project Page

- Description:&nbsp;Enables customization of a project’s overview page
- Author:&nbsp; [Finnlabs](http://www.finn.de/)
- Link: [Source](https://github.com/finnlabs/openproject-my_project_page)

```ruby
  gem 'openproject-my_project_page', :git => 'https://github.com/finnlabs/openproject-my_project_page.git', :branch => 'stable'
```

### PDF Export

- Description:&nbsp;Enables story card export for OpenProject Backlogs
- Author:&nbsp; [Finnlabs](http://www.finn.de/)
- Link: [Source](https://github.com/finnlabs/openproject-pdf_export)
- Dependency: Backlogs

```ruby
  gem 'openproject-pdf_export', :git => 'https://github.com/finnlabs/openproject-pdf_export.git', :branch => 'stable'
```

### Documents

- Description:&nbsp;Allows adding documents to projects
- Author:&nbsp; [OpenProject Foundation](https://www.openproject.org/projects/openproject/wiki/OpenProject_Foundation)
- Link: [Source](https://github.com/opf/openproject-documents)

```ruby
  gem 'openproject-documents', :git => 'https://github.com/opf/openproject-documents.git', :branch => 'stable'
```

### Reporting

- Description:&nbsp;Customized cost reporting features
- Author:&nbsp; [Finnlabs](http://www.finn.de/)
- Link: [Source](https://github.com/finnlabs/openproject-reporting)
- Dependency: Costs

```ruby
  gem 'reporting_engine', :git => 'https://github.com/finnlabs/reporting_engine.git', :branch => 'stable'
```

### Translations

- Description:&nbsp;Adds user contributed languages to your OpenProject installation. All translations come from&nbsp; [our crowdin project](https://crowdin.net/project/openproject)
- Author:&nbsp; [OpenProject Foundation](https://www.openproject.org/projects/openproject/wiki/OpenProject_Foundation)
- Link: [Source](https://github.com/opf/openproject-translations)

```ruby
  gem 'openproject-translations', :git => 'https://github.com/opf/openproject-translations.git', :branch => 'stable'
```

## Community Plugins

There are several plugins developed by dedicated community members. We really appreciate their hard work!

Please keep in mind that it can not always be guaranteed that they are compatible with the current OpenProject version. Authors are responsible for maintaining their plugins and are doing the best they can to keep the plugins up to date. &nbsp;Please contact the authors directly if you have any questions or feedback, or even want to help them out.

### Auto Project

- Description:&nbsp;Creates a project whenever a user is added
- Author:&nbsp; [Oliver Günther](https://github.com/oliverguenther)
- Link: [Source](https://github.com/oliverguenther/openproject-auto_project)

```ruby
  gem "openproject-auto_project", :git => "https://github.com/oliverguenther/openproject-auto_project.git", :branch => 'stable'
```

### Emoji

- Description:&nbsp;Adds emojis (emoticon) support to OpenProject
- Author:&nbsp; [Philipp Tessenow](https://github.com/tessi)
- Link: [Source](https://github.com/tessi/openproject-emoji)
- Dependency:

```ruby
  gem 'openproject-emoji',:git => 'https://github.com/tessi/openproject-emoji.git', :branch => 'stable'
```

### Ensure Project Hierarchy

- Description:&nbsp;Ensures subproject identifiers are prefixed with their parent project’s identifier
- Author:&nbsp; [Oliver Günther](https://github.com/oliverguenther)
- Link: [Source](https://github.com/oliverguenther/openproject-ensure_project_hierarchy)

```ruby
  gem "openproject-ensure_project_hierarchy", :git => "https://github.com/oliverguenther/openproject-ensure_project_hierarchy.git", :branch => 'stable'
```

## Plugins in development (potentially unstable)

### OpenProject Git Hosting

- Description:&nbsp;Provides extensive features for managing Git repositories within OpenProject
- Author:&nbsp; [Oliver Günther](https://github.com/oliverguenther)
- Link: [Source](https://github.com/oliverguenther/openproject-revisions_git)

```ruby
  gem "openproject-git_hosting", :git => "https://github.com/oliverguenther/openproject-git_hosting.git", :branch => "dev"
```

&nbsp;

&nbsp;
