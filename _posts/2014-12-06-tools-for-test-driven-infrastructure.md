---
layout: page
title: Tools for Test-Driven Infrastructure
tagline:
categories:
- blog
tags:
- puppet 
- beaker
- infrataster
- rspec
- serverspec
- tdd
---

Short summaries of some popular tools for test-driven 
infrastructure.

<h3>RSpec</h3>

<h4>Description</h4>

Testing tool for the Ruby programming language. 

<a href="http://rspec.info">http://rspec.info</a>

<h4>Typical Usage</h4>

* Test-driven development for Ruby
* Design through specification
* Creation of mocks and stubs

<h4>Code Example</h4>

```ruby
# cricket_spec.rb
require 'cricket'

describe Cricket, "#score" do
  it "returns 0 for maiden over" do
    cricket = Cricket.new
    6.times { cricket.run(0) }
    cricket.score.should eq(0)
  end
end
```

<hr />
<h3>RSpec-Puppet</h3>

<h4>Description</h4>

RSpec test tool for Puppet manifests.

<a href="http://rspec-puppet.com">http://rspec-puppet.com</a>

<h4>Typical Usage</h4>

* RSpec tests for Puppet modules
* Specification tests during module development

<h4>Code Example</h4>

```ruby
require 'spec_helper'

describe 'logrotate::rule' do
  let(:title) { 'nginx' }

  it { should contain_class('logrotate::rule') }

  context 'with compress => true' do
    let(:params) { {:compress => true} }

    it do
      should contain_file('/etc/logrotate.d/nginx') \
        .with_content(/^\s*compress$/)
    end
  end
end
```

<hr />
<h3>Beaker</h3>

<h4>Description</h4>

PuppetLabs acceptance testing tool.

<a href="https://github.com/puppetlabs/beaker">https://github.com/puppetlabs/beaker</a>

<h4>Typical Usage</h4>

* Puppet acceptance testing
* Virtual machines or containers can be provisioned for tests
* Tests run on the server via SSH

<h4>Example Scenarios</h4>

* Module should start a service
* Module should add a file "foo"
* Module should add a directory "bar"

<h4>Code Example</h4>

```ruby
require 'spec_helper_acceptance'

describe 'themodule::custom_file define' do
  context 'invalid file' do
    it 'should not add the file' do
      pp = <<-EOS
        class { 'themodule': }
        themodule::custom_file { 'acceptance_test':
          content => 'IS_INVALID',
        }
      EOS

      apply_manifest(pp, :expect_failures => true)
    end

    describe file("#{$config_dir}/acceptance_test.conf") do
      it { is_expected.not_to be_file }
    end
  end
end
```

<hr />
<h3>Infrataster</h3>

<h4>Description</h4>

Infrastructure behaviour testing framework

<a href="https://github.com/ryotarai/infrataster">https://github.com/ryotarai/infrataster</a>

<h4>Typical Usage</h4>

* System behaviour testing
* Test the external **behaviour** of a system
* Tests run from an external harness

<h4>Example Scenarios</h4>

* Valid web service response is returned
* SQL statement returns the correct results

<h4>Code Example</h4>
```ruby
require 'spec_helper'

describe server(:web) do
  describe http('http://web') do
    it "responds content including 'Hello World'" do
      expect(response.body).to include('Hello World')
    end
    it "responds OK 200" do
      expect(response.status).to eq(200)
    end
    it "responds as 'text/html'" do
      expect(response['content-type']).to eq('text/html')
    end
  end
end
```

<hr />
<h3>ServerSpec</h3>

<h4>Description</h4>

RSpec test tool for checking servers are configured correctly.

<a href="http://serverspec.org">http://serverspec.org</a>

<h4>Typical Usage</h4>

* Infrastructure installation testing
* Test the internal **state** of a configured server
* Tests run on the server via SSH

<h4>Example Scenarios</h4>

* Service installed
* User present
* Directory present

<h4>Code Example</h4>

```ruby
require 'spec_helper'

describe package('apache') do
  it { should be_installed }
end

describe service('apache') do
  it { should be_running   }
end

describe port(80) do
  it { should be_listening }
end
```

<hr />
<h3>Puppet-Lint</h3>

<h4>Description</h4>

Tests that a Puppet manifest conforms to a style guide

<a href="http://puppet-lint.com">http://puppet-lint.com</a>

<h4>Typical Usage</h4>

* Ensure that manifests comply with agreed coding styles

<h4>Example Scenarios</h4>

* Check for additional whitespace in code
* Ensure braces and brackets are correctly positioned
* Check for trailing newlines
