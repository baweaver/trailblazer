# 0.4.0

setup in initialize: when Op.run() with Worker, the policy will be run "delayed" and not with the actual permission set. this will result in many crashing sidekiq workers.

# 0.3.4

* Added `Operation::Policy`.
* Added `Operation::Resolver`.

# 0.3.3

* Add `Operation::reject` which will run the block when _invalid_.
* In the railtie, require `trailblazer/autoloading` as I am assuming Rails users want maximum comfort.

# 0.3.2

* Allow to use `#contract` before `#validate`. The contract will be instantiated once per `#run` using `#contract` and then memoized. This allows to add/modify the contract _before_ you validate it using `#validate`.
* New signature for `Operation#contract_for(model, contract_class)`. It used to be contract, then model.

# 0.3.1

* Autoload `Dispatch`.

# 0.3.0

## Changes

* In Railtie, use `ActionDispatch::Reloader.to_prepare` for autoloading, nothing else. This should fix spring reloading.
* Allow `Op#validate(params, model, Contract)` with CRUD.
* Allows prefixed table names, e.g. `admin.users` in `Controller`. The instance variables will be `@user`. Thanks to @fernandes and especially @HuckyDucky.
* Added `Operation::Collection` which will allow additional behavior like pagination and scoping. Thanks to @fernandes for his work on this.
* Added `Operation::collection` to run `setup!` without instantiating a contract. This is called in the new `Controller#collection` method.
* Added `Operation#model` as this is a fundamental concept now.
* Improved the undocumented `Representer` module which allows inferring representers from contract, using them to deserialize documents for the form, and rendering documents.
* Changed `Operation::Dispatch` which now provides imperative callbacks.

## API change

1. The return value of #process is no longer returned from ::run and ::call. They always return the operation instance.
2. The return value of #validate is `true` or `false`. This allows a more intuitive operation body.

    ```ruby
    def process(params)
      if validate(params)
        .. do valid
      else
        .. handle invalid
      end
    end
    ```

* `Worker` only works with Reform >= 2.0.0.

# 0.2.3


# 0.2.2

# Added Operation#errors as every operation maintains a contract.

# 0.2.1

* Added `Operation#setup_model!(params)` that can be overridden to add nested objects or process models right after `model!`. Don't add deserialization logic here, let Reform/Representable do that.
* Added `Operation#setup_params!(params)` to normalize parameters before `#process`. Thanks to @gogogarrett.
* Added `Controller::ActiveRecord` that will setup a named controller instance variable for your operation model. Thanks @gogogarrett!
* Added `CRUD::ActiveModel` that currently infers the contract's `::model` from the operation's model.

# 0.2.0

## API Changes

* `Controller#present` no longer calls `respond_to`, but lets you do the rendering. This will soon be re-introduced using `respond(present: true)`.
* `Controller#form` did not respect builders, this is fixed now.
* Use `request.body.read` in Unicorn/etc. environments in `Controller#respond`.

## Stuff

* Autoloading changed, again. We now `require_dependency` in every request in dev.

# 0.1.3

* `crud_autoloading` now simply `require_dependency`s model files, then does the same for the CRUD operation file. This should fix random undefined constant problems in development.
* `Controller#form` did not use builders. This is fixed now.

# 0.1.2

* Add `crud_autoloading`.

# 0.1.1

* Use reform-1.2.0.

# 0.1.0

* First stable release after almost 6 months of blood, sweat and tears. I know, this is a ridiculously brief codebase but it was a hell of a job to structure everything the way it is now. Enjoy!