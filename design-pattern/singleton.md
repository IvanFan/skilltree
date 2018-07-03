```py
class Singleton:
   __instance = None
   __use_global = False
   def __new__(cls, **kwargs):
       if Singleton.__use_global:
          if Singleton.__instance is None:
              Singleton.__instance = object.__new__(cls)
          instance = Singleton.__instance
      else:
          instance = object.__new__(cls)
      return instance

  @classmethod
  def enable_global(cls):
      Singleton.__use_global = True


  @classmethod
  def get_instance(cls):
      return cls.__instance
```



