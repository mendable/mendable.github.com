--- 
layout: post
title: Polymorphic Single Table Inheritance (STI)
wordpress_id: 244
wordpress_url: http://www.mendable.com/?p=244
---
Migration:
{% highlight ruby %}
  create_table :addresses do |t|
    ....
    t.references :addressable, :polymorphic => true
    t.string :type
    t.timestamps
  end
{% endhighlight %}

Address Models:
{% highlight ruby %}
class Address < ActiveRecord::Base
    belongs_to :addressable, :polymorphic => true
end
class InvoiceAddress < Address
end
class DeliveryAddress < Address
end
{% endhighlight %}

User Model:
{% highlight ruby %}
class User < ActiveRecord::Base
  has_many :addresses, :dependent => :destroy
  has_one :invoice_address, :as => :addressable
  has_one :delivery_address, :as => :addressable
end
{% endhighlight %}

Order Model:
{% highlight ruby %}
class Order < ActiveRecord::Base
  has_many :addresses, :dependent => :destroy
  has_one :invoice_address, :as => :addressable
  has_one :delivery_address, :as => :addressable
end
{% endhighlight %}

Get Funky:
{% highlight irb %}
>> u = User.new
=> <User id: nil ...>
>> u.build_invoice_address
=> #<InvoiceAddress id: nil, addressable_id: nil, addressable_type: "User", 
created_at: nil, updated_at: nil, type: "InvoiceAddress">
>> u.build_delivery_address
=> <DeliveryAddress id: nil, addressable_id: nil, addressable_type: "User", 
created_at: nil, updated_at: nil, type: "DeliveryAddress">
>> o = Order.new
=> #<Order id: nil ...>
>> o.build_invoice_address
=> #<InvoiceAddress id: nil, addressable_id: nil, addressable_type: "Order", 
created_at: nil, updated_at: nil, type: "InvoiceAddress">
{% endhighlight %}

Sweet :)
