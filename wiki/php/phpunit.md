# Mock

## Prophesize

Example:

```php
$gateway = $this->prophesize(Example\Gateway::class);
$gateway->findAllExampleEntity(\Prophecy\Argument::any())->willReturn([$entity1, $entity2]);
$service = new Example\Service($gateway->reveal());
// \Prophecy\Argument::type(\Fiscal\Model\NFE::class) <-- type
```

## MockBuilder

Example:

```php
$gateway = $this->getMockBuilder(DownloadGateway::class)
    ->setConstructorArgs([])
    ->setMethods(['getNFEByNumber'])
    ->getMock();
$gateway->expects($this->once())
    ->method('getNFEByNumber')
    ->willReturn([$nfe1, $nfe2]);
```

Mock que valida se entro no metodo e quais foram os parametros passados

```php
$gateway = $this->getMockBuilder(NFEGateway::class)
    ->setMethods(['findNFEsByEmissorAndModelAndDates'])
    ->disableOriginalConstructor()
    ->getMock();
$gateway->expects($this->once())
    ->method('findNFEsByEmissorAndModelAndDates')
    ->with(
        $this->identicalTo($emissor),
        $this->identicalTo($params['modelo']),
        $this->callback(function ($from) { return $from === '2016-11-01'; }),
        $this->callback(function ($to) { return $to === '2016-11-10'; }),
        $this->identicalTo(20),
        $this->identicalTo(0)
    )
    ->willReturn([]);
```

## Exception

```php
/**
 * @expectedException MyException
 * @expectedExceptionCode 20
 * @expectedExceptionMessage Some Message
 */
public function testExceptionHasErrorcode20()
{
    throw new MyException('Some Message', 20);
}
```
