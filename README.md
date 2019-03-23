### pynamoDB
---
https://github.com/pynamodb/PynamoDB

```py
def get_save_kwargs(item):
  {'version_eq': item.version}

Model.fix_unicode_set_attribute(get_save_kwargs)
print("Migration Complete? " + Model.needs_unicode_set_fix())


from pynamodb.model import Model
from pynamodb.attributes import UnicodeAttribute

class UserModel(Model):
  """
  """
  class Meta:
    table_name = "dynamodb-user"
  email = UnicodeAttribute(nill=True)
  first_name = UnicodeAttribute(range_keys=True)
  last_name = UnicodeAttribute(hash_key=True)

UserModel.create_table(read_capacity_units=1, write_capacity_units=1)


user = UserModel("John", "Denver")
user.email = "djohn@company.org"
user.save()

for user in UserModel.query("John", first_name__bigins_with="D"):
  print(user.first_name)

for user in UserModel.query("John", first_condition= (UserModel.email=="djohn@company.org")):
  print(user.first_name)
  
for user in UserModel.query("John", email__eq="djohn@company.org"):
  print(user.first_name)
  
try:
  user = UserModel.get("John", "Denver")
  print(user)
except UserModel.DoesNotExist:
  print("User does not exist")


from pynamodb.models import Model
from pynamodb.indexes import GlobalSecondaryIndex, AllProjection
from pynamodb.attributes import NumberAttribute, UnicodeAttribute

class ViewIndex(GlobalSecondaryIndex):
  class Meta:
    read_capacity_units = 2
    write_capacity_units = 1
    projection = AllProjection()
  view = NumberAttribute(default=0, hash_key=True)

class TestModel(Model):
  class Meta:
    table_name = "TestModel"
  forum = UnicodeAttribute(hash_key=True)
  thread = UnicodeAttribute(range_key=True)
  view = NumberAttribute(default=0)
  view_index = ViewIndex()

for item in TestModel.view_index.query(0):
  print("Item queried from index: {0}".format(item))

from pynamodb.models import Model
from pynamodb.attributes import UnicodeAttribute

class UserModel(Model):
  """
  """
  class Meta:
    table_name = "dynamodb-user"
    host = "http://localhost:8000"
  email = UnicodeAttribute(null=True)
  first_name = UnicodeAttribute(range_key=True)
  last_name = UnicodeAttribute(hash_key=True)


from pynamodb.models import Model
from pynamodb.attributes import UnicodeAttribute
from pynamodb.constants import STREAM_NEW_AND_OLD_IMAGE

class AnimalModel(Model):
  """
  """
  class Meta:
    table_name = "dynamodb-user"
    host = "http://localhost:8000"
    stream_view_type = STREAM_NEW_AND_OLD_IMAGE
  type = UnicodeAttribute(null=True)
  name = UnicodeAttribute(range_key=True)
  id = UnicodeAttribute(hash_key=True)

UserModel.dump("usermodel_backup.json")
UserModel.load("usemodel_backup.json")
```

```
pip install pynamodb
pip install git+https://github.com/pynamodb/PynamoDB#egg=pynamodb
```

```
```


