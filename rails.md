# Rails

## Generators
```sh
# no test framework flags in generators
--no-test-framework
# no helper
--no-helper
# no assets
--no-assets

# scaffold with no test framework
rails g scaffold name model_name:belongs_to{} column_name:column_data_type{size}:index:uniq --no-helper --no-assets --no-controller-specs --no-view-specs --no-test-framework

# model generation with no test framework
rails g model model_name --no-test-framework

# controller generation with no test frame work
rails g controller controller_name action_name --no-helper --no-assets --no-controller-specs --no-view-specs --no-test-framework

## Examples

# Example 1
# scaffold for person with company references/belongs_to, and columns:
# company:references 
# token:string:index 
# name:string 
# email:string:index 
# phone:string:index 
# designation:string 
# age:integer 
# gender:string{2} 
# religion:string 
# nationality:string 
# education:string 
# qualification:string 
# website:text 
# marital_status:string 
# categories:text 
# address:text 
# summary:text 
# experience:string 
# key:boolean 
# rating:decimal, 
# last_refersh_at:datetime
rails g scaffold person company:references token:string:index name:string email:string:index phone:string:index designation:string age:integer gender:string{2} religion:string nationality:string education:string education:string qualification:string website:text marital_status:string categories:text address:text address_1:text summary:text experience:string key:boolean rating:decimal, last_refersh_at:datetime --no-helper --no-assets --no-controller-specs --no-view-specs --no-test-framework

# Example 2
# scaffold for profile with profilable references/polymorphic, and columns:
# profilable:references{polymorphic} 
# source:references 
# url:text 
# url_hash:string:index 
# raw:text
rails g scaffold profile profilable:references{polymorphic} source:references url:text url_hash:string:index raw:text --no-helper --no-assets --no-controller-specs --no-view-specs --no-test-framework

# Example 3
# generate model restaurant with product_detail references and columns
# product_detail:belongs_to 
# rating:decimal 
# review:text
rails g model restaurant product_detail:belongs_to rating:decimal review:text --no-test-framework

# Example 4
# generate dashboard controller with index action
rails g controller dashboard index --no-helper --no-assets --no-controller-specs --no-view-specs --no-test-framework
```