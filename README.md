flow
====

Declarative flows for Rails with an emphasis on testability.

Usage
=====

In your `Gemfile`:

```ruby
gem 'flow'
```

In a controller:

```ruby
class SignupFlow < ApplicationController

  Flow.define name: :signup_flow, path: '/signup', controller: self do |flow|
  
    flow.step name: :start, path: '', next_step: :another_step do |step|
      step.get :start do
      end
      
      step.post :submit_start do
        # Do something, then...
        redirect_to :another_step
      end
    end
    
    flow.step name: :another_step, path: 'about-you', next_step: :finish do |step|
      step.get :another_step do
      end
      
      step.post :submit_another_step do
        # Do some stuff, then...
        redirect_to :finish
      end
    end
    
    flow.step name: :finish, path: 'thank-you' do |step|
      step.get :finish do
      end
    end
    
  end

end

```

In `spec/controllers/signup_flow_spec.rb`:

```ruby
require 'spec_helper'

describe SignupFlow do
  describe :start do
    before do
      get :start
    end
    it { should render_template :start }
  end
  
  describe :submit_start do
    before do
      post :submit_start
    end
    it { should redirect_to :another_step }
  end
  
  describe :another_step do
    before do
      get :another_step
    end
    it { should render_template :another_step }
  end
  
  describe :submit_another_step do
    before do
      post :submit_another_step
    end
    it { should redirect_to :finish }
  end
  
  describe :finish do
    before do
      get :finish
    end
    it { should render_template :finish }
  end
end
```

