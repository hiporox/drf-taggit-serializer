# Django Taggit Rest Serializer

## About
This package is meant for the `django-taggit` package which is available [here](https://github.com/alex/django-taggit)

The `django-taggit` package makes it possible to tag a certain module.

## Installation
To install this package you can use the following `pip` installation:
```
pip install drf-taggit-serializer
```

Then, add `taggit_serializer` to your `Settings` in `INSTALLED_APPS`:
```
    INSTALLED_APS = (
        ...
        'taggit_serializer',
    )
```

## Usage
Because the tags in `django-taggit` need to be added into a `TaggableManager()` we cannot use the usual `Serializer` that we get from Django REST Framework. Because this is trying to save the tags into a `list`, which will throw an exception.

To accept tags through a `REST` API call we need to add the following to our `Serializer`:
```python
from taggit_serializer.serializers import (TagListSerializerField,
                                           TaggitSerializer)


class YourSerializer(TaggitSerializer, serializers.ModelSerializer):

    tags = TagListSerializerField()

    class Meta:
        model = YourModel
```

And you're done, so now you can add tags to your model

## Troubleshooting

### Serializer is working but not persistent on DB
Probably, yo've used the intheritance classes in the wrong order.
The order on `YourSerializer` class inheritance classes is imporant, cause you have to override `to_internal_value` by django serializer mixins. So, use `TaggitSerializer` as the first class.

```
class YourSerializer(serializers.ModelSerializer, TaggitSerializer): # Using wrong to_internal_value function
class YourSerializer(TaggitSerializer, serializers.ModelSerializer): # Replace with this
```

## Contribute

Please feel free to create pull requests and issues!
Please try to follow [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) to make it easier to track what changes you have made.


## Credits

This is a fork of https://github.com/glemmaPaul/django-taggit-serializer. Many thanks to [@glemmaPaul](https://github.com/glemmaPaul) for managing that repo.
