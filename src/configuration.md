title: Configuration
---

## Configuration

This class name is not really *reserved* within Rails. It's more to do with it conflicting with a class already within Rails. If you attempt to reference `Configuration` inside a controller action:

    def index
      @configurations = Configuration.scoped
    end

You'll see this output:

    undefined method `scoped' for ActiveSupport::Configurable::Configuration:Class


This is not the class you're looking for! This is happening because `ActiveSupport::Configurable` is one of the modules included into `ActionController::Base`, and that module contains a class called `Configuration`.

 You can work around this problem by prefixing the class name with `::`, which is the Ruby shortcut to tell it to look in the top-level namespace, or you could simply rename `Configuration` to something else, such as `Setting`.


