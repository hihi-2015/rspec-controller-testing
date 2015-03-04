#A template for Rails Rspec controller testing
---

We have used a generic "ItemController" and a subsequent "Item" model, with 'item' objects. Update your controller test to include the appropriate model/controller names.

---
##\#New Action
This tests the #new action in the controller, we provide some examples for tests, both hitting the database and not.

###Context: with hitting the database
```ruby
before do
  get :new
end

it "creates a new item" do
  expect(assigns(:item)).to be_a_new(Item)
end
```

###Context: without hitting the database
```ruby
let(:item_double) { double("item_double") }
before(:each) do
  allow(Item).to receive(:new).and_return(item_double)
  get :new
end

it "creates a new item" do
  expect(assigns(:item)).to be(item_double)
end
```
---
##\#Create Action
This tests the #create action in the controller, we provide some examples for tests, both hitting the database and not.

###Context: with hitting the database
```ruby
it "creates a new Item" do
  expect {
    post :create, item: FactoryGirl.attributes_for(:item)
  }.to change(Item, :count).by(1)
end
```
###Context: without hitting the database
```ruby
let(:item_double) { double("item_double") }

before do
  allow(Item.stub).to receive(:new).and_return(item_double)
  allow(item_double).to receive(:save).and_return(true)
end

it "creates a new item" do
  post :create
  expect(assigns(:item)).to be(item_double)
end

it "redirects to the correct url" do
  post :create
  expect(response).to redirect_to root_url
end
```
---
##\#Edit Action
This tests the #edit action in the controller, we provide some examples for tests, both hitting the database and not.

###Context: with hitting the database
```ruby
let(:item) { Item.create(first_attribute: "My name", second_attribute: 23) }

before do
  get :edit, id: item
end

it "finds a specific item" do
  expect(assigns(:item)).to eq(item)
end
```
###Context: without hitting the database
```ruby
let(:item) { double("item") }

it "finds a specific item" do
  get :edit, id: item
  expect(Item).to recieve(:find).once.and_return(item)
end
```
---
###Update Action
This tests the #update action in the controller, we provide some examples for tests, both hitting the database and not.

###Context: with hitting the database
```ruby
let(:item) { Item.create(first_attribute: "My name", second_attribute: 23) }

it "updates an item with valid params" do
  post :update, id: item, item: { first_attribute: "Updated name", second_attribute: 23 }
  item.reload
  expect(item.first_attribute).to eq("Updated name")
end
```
###Context: without hitting the database
```ruby
let(:item) { double("todo") }
let(:attrs) { first_attribute: "Updated name", second_attribute: 23 }

it "updates an item with valid params" do
  allow(Item).to receive(:find).and_return(item)
  expect(item).to recieve(:update_attributes).with(attrs.stringify_keys)
  post :update, id: item, item: attrs
end

it "redirects to item once updated" do
  item = stub_model(Item)
  allow(Item).to receive(:find).and_return(item)
  allow(item).to receive(:update_attributes).and_return(true)
  post :update, id: item, item: attrs
  expect(response).to redirect_to(item)
end
```
















