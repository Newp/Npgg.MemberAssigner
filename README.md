# Npgg.MemberAssigner


# 특징
1. Reflection 으로 type에 대한 Member(Field/Property)의 값을 get/set 할 수 있습니다.
2. 내부적으로 Expression Tree를 사용하여 훨씬 빠른 속도를 보장합니다.
3. 자체적으로 캐싱 기능을 가지고 있기 때문에 반복적인 사용환경에서 더 높은 성능을 보장합니다.
4. Member 에 대한 FieldMember/PropertyMember를 구분할 필요가 없이 사용할 수 있어 보다 간편합니다.


# Reflection 과 성능비교
set value via System.Reflection  1810 ms elapsed
set value via Npgg.MemberAssigner 76 ms elapsed


# 편리한 사용

###set value via Npgg.MemberAssigner
```csharp
void SampleAssign(object instance, MemberInfo memberInfo, string value)
{
    var assigner = new MemberAssigner(memberInfo);
    assigner.SetValue(instance, value);
}
```

###set value via Npgg.MemberAssigner
```csharp
// MemberInfo의 타입이 FieldInfo/PropertyInfo인지에 따라 처리를 달리해야 함
void SampleAssignByReflection(object instance, MemberInfo memberInfo, string value)
{
    if(memberInfo is FieldInfo fieldInfo) 
    {
        fieldInfo.SetValue(instance, value);
    }
    else if(memberInfo is PropertyInfo propertyInfo)
    {
        propertyInfo.SetValue(instance, value);
    }
}

```
# MemberAssignerPool

한번 초기화했던 타입에 대해서 자동으로 캐싱합니다. 

### Cache 사용여부에 따른 성능 비교 
without cache 6045 ms elapsed, with cache 1 ms elapsed



## 한번에 해당 타입의 모든 Assigner 가져오기

```csharp
var assigners = MemberAssigner.GetAssigners(item.GetType());
```
또는,
```csharp
var assigners = MemberAssigner.GetAssigners<TYPE>();
```
