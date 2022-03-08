# laminas-container-config-test

[![Build Status](https://github.com/laminas/laminas-container-config-test/workflows/Continuous%20Integration/badge.svg)](https://github.com/laminas/laminas-container-config-test/actions?query=workflow%3A"Continuous+Integration")

> ## 🇷🇺 Русским гражданам
> 
> Мы, участники Laminas, родились и живем в разных странах. У многих из нас есть друзья, родственники и коллеги как в России, так и в Украине. Некоторые из нас родились в России. Некоторые из нас живут в России. У некоторых бабушки и дедушки сражались с фашистами во Второй мировой войне. Здесь никто не поддерживает фашизм.
> 
> У одного из нас есть украинская родственница, которая спаслась из дома вместе с сыном. Поезд задержался из-за бомбежки на дороге впереди. У нас есть друзья, которые прячутся в бомбоубежищах. Мы с тревогой ждем весточки от них после воздушных налетов, которые беспорядочно наносят удары и попадают по больницам, школам, детским садам и домам. Мы не берем это из каких-либо СМИ. Мы наблюдаем это напрямую.
> 
> Вы доверяете нам достаточно, чтоб использовать наши программы, и мы просим вас довериться нам вновь. Мы нуждаемся в помощи. Выходите и протестуйте против этой бесполезной войны. Остановите кровопролитие. Скажите "Нет войне!"
> 
> ## 🇺🇸 To Citizens of Russia
> 
> We at Laminas come from all over the world. Many of us have friends, family and colleagues in both Russia and Ukraine. Some of us were born in Russia. Some of us currently live in Russia. Some have grandparents who fought Nazis in World War II. Nobody here supports fascism.
> 
> One team member has a Ukrainian relative who fled her home with her son. The train was delayed due to bombing on the road ahead. We have friends who are hiding in bomb shelters. We anxiously follow up on them after the air raids, which indiscriminately fire at hospitals, schools, kindergartens and houses. We're not taking this from any media. These are our actual experiences.
> 
> You trust us enough to use our software. We ask that you trust us to say the truth on this. We need your help. Go out and protest this unnecessary war. Stop the bloodshed. Say "stop the war!"

This library provides common tests for PSR-11 containers configured using a
subset of [laminas-servicemanager](https://github.com/laminas/laminas-servicemanager)
[configuration](https://docs.laminas.dev/laminas-servicemanager/configuring-the-service-manager/)
as [specified by Mezzio](https://docs.mezzio.dev/mezzio/v3/features/container/config/)

It guarantees delivery of the same basic functionality across multiple PSR-11
container implementations, and simplifies switching between them.

Currently we support:
- [Aura.Di](https://github.com/auraphp/Aura.Di) - via [laminas-auradi-config](https://github.com/laminas/laminas-auradi-config)
- [Pimple](https://pimple.symfony.com/) - via [laminas-pimple-config](https://github.com/laminas/laminas-pimple-config)
- [laminas-servicemanager](https://github.com/laminas/laminas-servicemanager)

## Installation

Run the following to install this library:

```bash
$ composer require --dev laminas/laminas-container-config-test
```

## Using common tests

In your library, you will need to extend the
`Laminas\ContainerConfigTest\AbstractContainerTest` class within your test suite and
implement the method `createContainer`:

```php
protected function createContainer(array $config) : ContainerInterface;
```

It should return your PSR-11-compatible container, configured using `$config`.

Then, depending on what functionality you'd like to support, you can add the
following traits into your test case:

- `Laminas\ContainerConfigTest\AliasTestTrait` - to support `aliases` configuration,
- `Laminas\ContainerConfigTest\DelegatorTestTrait` - to support `delegators` configuration,
- `Laminas\ContainerConfigTest\FactoryTestTrait` - to support `factories` configuration,
- `Laminas\ContainerConfigTest\InvokableTestTrait` - to support `invokables` configuration,
- `Laminas\ContainerConfigTest\ServiceTestTrait` - to support `services` configuration,
- `Laminas\ContainerConfigTest\SharedTestTrait` - to support `shared` and `shared_by_default` configuration.

To provide an Mezzio-compatible container, you should extend the class
`Laminas\ContainerConfigTest\AbstractMezzioContainerConfigTest`
and implement the method `createContainer`. This class composes the following traits:

- `Laminas\ContainerConfigTest\AliasTestTrait`,
- `Laminas\ContainerConfigTest\DelegatorTestTrait`,
- `Laminas\ContainerConfigTest\FactoryTestTrait`,
- `Laminas\ContainerConfigTest\InvokableTestTrait`,
- `Laminas\ContainerConfigTest\ServiceTestTrait`.

If you want also plan to support shared services, your test class should compose
the `SharedTestTrait` as well:

```php
use Laminas\ContainerConfigTest\AbstractMezzioContainerConfigTest;
use Laminas\ContainerConfigTest\SharedTestTrait;

class ContainerTest extends AbstractMezzioContainerConfigTest
{
    use SharedTestTrait;
    
    protected function createContainer(array $config) : ContainerInterface
    {
        // your container configuration
    }
}
```
