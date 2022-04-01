### There are no currency conversion plugins or templates available for #redmine. So we have come up with our own tweak. This will be useful for #financemanagement and #pmos to quickly do currency conversion in the tracker. This plugin extension fetches current currency rate and gives the conversion.

## Steps for using this partial
1. [Get your free API access key from here](@https://www.exchangerate-api.com/)  
2. Put this partial in the view folder.
3. Render this partial by adding following line into the place where you want the conversion happen

```
<%= render "controller_name/currency_conversion_partial"%>
```
4. Save the file
5. Migrate and restart the server.
6. Enjoy



## Adding role based access( in Progress)

Way 2
steps:

1. add a column to roles table
```
 bundle exec rails generate redmine_plugin_migration redmine_wktime add_showcurrencyconverter_to_roles showcurrencyconverter:boolean
```
2.This should be the  migration file
```
class AddShowcurrencyconverterToRoles < ActiveRecord::Migration[5.2]
  def change
    add_column :roles, :show_currency_converter ,:boolean
  end
end
```
3.db migration 
```
bundle exec rake redmine:plugins:migrate NAME=redmine_wktime RAILS_ENV=development
```
3.View partial
```

<%
@role=Role.all
access_hash={}
@role.each do |role|
  access_hash.merge!(role.name => "#{role.show_currency_converter}")
end

puts access_hash

%>
<p>
<%= access_hash %>
</p>



<script>
function getdata(){
    var manager=document.getElementById("manager");
    var developer=document.getElementById("developer");
    var reporter=document.getElementById("reporter");

    //this will return true or false
    document.getElementById("show").innerHTML = manager.checked;

    //this will return value 
    //document.getElementById("show").innerHTML = manager.value;
}
</script>
<div>
<fieldset>
<legend><%= l(:label_currency_conversion) %></legend><br/>
<h3>Visible to</h3>
<input type="checkbox" id="manager" name="manager" value="manager" onchange="getdata()"> Manager
<br/>
  <input type="checkbox" id="developer" name="developer" value="developer" onchange="getdata()">  Developer 
<br/>
  <input type="checkbox" id="reporter" name="reporter" value="reporter" onchange="getdata()"> Reporter 

<p id="show"> </p>
</div>
</fieldset>
```
New code 

```
<fieldset>
  <div>
  <legend><%= l(:label_wk_currency_converter) %></legend><br/>
  <h3>Visible To</h3>
  <% @role=Role.all %>
    <%  @role.each do |role| %>
      <%#= check_box(role, checked_value = "1", unchecked_value = "0")%>
            <input type="checkbox" class="currency_converter_checkboxes" id="<%= role.name %>" name="<%= role.name %>" value="<%= role.show_currency_converter %>"  > <%= role.name %>
      <br/>
    <%  end %>
    <button onclick="access_controller()">save</button>
  </div>
</fieldset>
<script>
    function access_controller(){
        alert("access_controller called");
        //1-checked attribute according to a varible
        //2- checkbox change register
        //3- save change to db
        <%= @role.each do |role| %>
          
        <% end %>



    }
</script>




```
