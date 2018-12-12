# Commands

Prefixo instalado via composer: `php vendor/bin/doctrine *`

* `orm:schema-tool:create` - Create database
* `orm:schema-tool:drop --force` - Delete database
* `orm:schema:update --dump-sql` - Dump or update modifications
* `orm:validate-schema` - Validate Entity with DataBase
* `orm:convert-mapping --force  --from-database annotation [path]` - Generate entities by database
* `orm:generate-entities [path] --generate-annotations=true` - Update entities and generate entities with annotations, get and set

# Persisting Usage

### - Persisting objects

```php
$user1 = new User();
$user1->setFullName('MyName');
$em->persist($user1);

$user2 = new User();
$user2->setFullName('MyName2');
$em->persist($user2);

$em->flush();
```

### - Updating an object

```php
$user = $em->find('User', 1);
$user->setName('MyName');
$em->flush();
```

### - Deleting an object

```php
$user = $em->find('User', 1);
$em->remove($user);
$em->flush();
```

# Retrieving an object

## - Repositories

### Single user by id
```php
$user1 = $em->find('User', 1);
$user2 = $em->getRepository('User')->find(1);
```

### Single user by name and type.
```php
$user = $em->getRepository('User')->findOneBy(
    array('name' => 'MyName', 'type' => 'MyType'));
```

### Single user by its Name
```php
$user = $em->getRepository('User')->findOneByName('MyName');
```

### Find All users
```php
$users = $em->getRepository('User')->findAll();
```

### Group users by type
```php
$users = $em->getRepository('User')->findByType('MyType');
```

### Group users by criteria, order, limit, offset.
```php
$users = $em->getRepository('User')->findBy(
    array('name' => 'MyName'), array('id' => 'ASC'), 10, 0);
```

## - DQL

## - Query Builder

### Example BETWEEN

```php
$queryBuilder = $em->createQueryBuilder();
$queryBuilder->select('n')
    ->from(Example\Entity::class, 'n')
    ->where('n.date BETWEEN :from AND :to')

$queryBuilder->setParameters([
    'from' => $from,
    'to' => $to
]);

$result = $queryBuilder->getQuery()->fetchAll();
```

# Associations

## - One to One

### Mapping One to One

```php
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="user")
 */
class User
{
    /**
     * @ORM\OneToOne(targetEntity="Entity\Role", mappedBy="role", cascade={"persist"})
     */
    protected $role;
}

/**
 * @ORM\Entity
 * @ORM\Table(name="role")
 */
class Role
{
    /**
     * @ORM\OneToOne(targetEntity="Entity\User", inversedBy="role", cascade={"persist"})
     * @ORM\JoinColumn(name="userId", referencedColumnName="id", unique=false, nullable=false)
     */
    protected $user;
}
```

### Usage example One to One 

```php
//persist
$user = new Entity\User();
$user->setEmail('email');
$user->setName('name');

$role = new Entity\Role;
$role->setName('name');

$role->setUser($user);
$user->setRole($role);

$em->persist($user);
$em->flush();

//retrieving
$user = $em->find('User', $user->getId());
$user->getRole();
```

## - One to Many

### Mapping One to Many

```php
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="user")
 */
class User
{
    /**
     * @ORM\OneToMany(targetEntity="Comment", mappedBy="user", cascade={"persist"})
     */
    protected $comments;

    public function __construct()
    {
        $this->comments = new \Doctrine\Common\Collections\ArrayCollection();
    }
}

/**
 * @ORM\Entity
 * @ORM\Table(name="comment")
 */
class Comment
{
    /**
     * @ORM\ManyToOne(targetEntity="User", inversedBy="comments", cascade={"persist"})
     * @ORM\JoinColumn(name="userId", referencedColumnName="id", unique=false, nullable=false)
     */
    protected $user;
}
```

### Usage example One to Many 

```php
//persist
$user = new Entity\User();
$user->setEmail('email');
$user->setName('name');

$comment1 = new Entity\Comment;
$comment1->setCommentText('comment1');
$comment1->setUser($user);
$comment2 = new Entity\Comment;
$comment2->setCommentText('comment2');
$comment2->setUser($user);

$user->getComments()->add($comment2);
$user->getComments()->add($comment1);

$em->persist($user);
$em->flush();

//retrieving
$user = $em->find('User', $user->getId());
$user->getComments()->toArray();
```

### Mapping One to Many Self Ref

```php
class Comment
{
    /**
     * @ORM\OneToMany(targetEntity="Comment", mappedBy="parent", cascade={"persist"})
     */
    protected $children;

    /**
     * @ORM\ManyToOne(targetEntity="Comment", inversedBy="children", cascade={"persist"})
     * @ORM\JoinColumn(name="parentId", referencedColumnName="id", unique=false, nullable=true)
     */
    protected $parent;

    public function __construct()
    {
        $this->children = new \Doctrine\Common\Collections\ArrayCollection();
    }
}
```

### Usage example One to Many Self Ref

```php
// persisting
$comment2 = new Entity\Comment;
$comment2->setCommentText('parent');

$comment1 = new Entity\Comment;
$comment1->setCommentText('children1');
$comment1->setParent($comment2);
$comment3 = new Entity\Comment;
$comment3->setCommentText('children2');
$comment3->setParent($comment2);

$comment2->getChildrens()->add($comment1);
$comment2->getChildrens()->add($comment3);

$em->persist($comment2);
$em->flush();

//retrieving
$comment = $em->find('Comment', $comment2->getId());
$comment->getChildrens();
```

## - Many to Many

### Mapping Many to Many

```php
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="user")
 */
class User
{
    /**
     * @ORM\ManyToMany(targetEntity="Category", inversedBy="users", cascade={"persist"})
     * @ORM\JoinColumn(name="categoryId", referencedColumnName="id", unique=false, nullable=true)
     */
    protected $categories;

    public function __construct()
    {
        $this->categories = new \Doctrine\Common\Collections\ArrayCollection();
    }
}

/**
 * @ORM\Entity
 * @ORM\Table(name="category")
 */
class Category
{
    /**
     * @ORM\ManyToMany(targetEntity="User", mappedBy="categories", cascade={"persist"})
     */
    protected $users;

    public function __construct()
    {
        $this->users = new \Doctrine\Common\Collections\ArrayCollection();
    }
}
```

### Usage example Many to Many

```php
// persisting
$user1 = new Entity\User();
$user1->setEmail('email');
$user1->setName('Plumber Builder');
 
$user2 = new Entity\User;
$user2->setEmail('email');
$user2->setName('Plaster Builder');

$categoryPlumber = new Entity\Category;
$categoryPlumber->setName('Plumber Category');
$categoryBeer_Drinkers = new Entity\Category;
$categoryBeer_Drinkers->setName('Beer_Drinkers Category');

$categoryPlumber->getUsers()->add($user1);
$categoryBeer_Drinkers->getUsers()->add($user2);
$categoryBeer_Drinkers->getUsers()->add($user1);

$user1->getCategories()->add($categoryPlumber);
$user1->getCategories()->add($categoryBeer_Drinkers);
$user2->getCategories()->add($categoryBeer_Drinkers);

$em->persist($user1);
$em->persist($user2);
$em->flush();

//retrieving
$user1 = $em->find('User', $user1->getId());
//get last categories for user1
$user1->getCategories()->toArray());
//get last category(Beer_Drinkers Category) and all users for this category
$user1->getCategories()->last()->getUsers()->toArray();
```

# Lifecycle callbacks

Annotation which has to be set on the entity-class PHP DocBlock to notify Doctrine that this entity has entity lifecycle callback annotations set on at least one of its methods. Using @PostLoad, @PrePersist, @PostPersist, @PreRemove, @PostRemove, @PreUpdate or @PostUpdate without this marker annotation will make Doctrine ignore the callbacks.

```php
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\MappedSuperclass
 * @ORM\HasLifecycleCallbacks
 */
abstract class AbstractEntity
{
    /**
     * @ORM\Column(name="updated_at", type="datetime")
     */
    private $updatedAt;

    /**
     * @ORM\PrePersist()
     * @ORM\PreUpdate
     */
    public function preUpdated()
    {
        $this->updated = new \DateTime();
    }

    /**
     * @PostPersist
     */
    public function sendOptinMail() {}
}

/**
 * @ORM\Entity
 * @ORM\Table(name="user")
 */
class User extends Abstract
{
    // [...]
}
```

# Custom Repository

Entity mapping for repository class

```php
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity(repositoryClass="Repository\Comment")
 */
class Comment {
    // [...]
}

//repository Class
use Doctrine\ORM\EntityRepository;

class Comment extends EntityRepository
{
    public function findUserComments($userId)
    {
        $query = $this->_em->createQuery("SELECT u FROM Entity\Comment u WHERE u.userId = :userId");
        $query->setParameters(array('userId' => $userId));
        return $query->getResult();
    }
}
```

In controller

```php
$repository = $em->getRepository(Entity\Comment::class);
$comments = $repository->findUserComments(10);
```

# Zend Hydrator

```php
use Doctrine\ORM\Mapping as ORM;
use Zend\Hydrator;

/**
 * @ORM\Entity
 * @ORM\Table(name="user")
 */
class User
{
    // [...]

    public function hydrator(array $options) : User
    {
        $hydrator = new Hydrator\ClassMethods();
        $hydrator->hydrate($options, $this);
        return $this;
    }

    public function extract() : array
    {
        $hydrator = new Hydrator\ClassMethods();
        return $hydrator->extract($this);
    }
}
```
# My Entities

### - Abstract Entity

```php
namespace Core\Entity;

use Doctrine\ORM\Mapping as ORM;
use Zend\Hydrator;

/**
 * @author Jhon Mike <developer@jhonmike.com.br>
 *
 * @ORM\MappedSuperclass
 * @ORM\HasLifecycleCallbacks
 */
abstract class AbstractEntity
{
    /**
     * @ORM\Id
     * @ORM\Column(type="integer", nullable=false)
     * @ORM\GeneratedValue(strategy="IDENTITY")
     */
    private $id;

    /**
     * @ORM\Column(type="datetime", nullable=false)
     */
    private $created;

    /**
     * @ORM\Column(type="datetime", nullable=false)
     */
    private $updated;

    public function getId() : int
    {
        return $this->id;
    }

    public function getCreated() : DateTime
    {
        return $this->created;
    }

    /**
     * @ORM\PrePersist
     */
    public function setCreated()
    {
        $this->created = new \DateTime();
    }


    public function getUpdated() : DateTime
    {
        return $this->updated;
    }

    /**
     * @ORM\PrePersist
     * @ORM\PreUpdate
     */
    public function setUpdated()
    {
        $this->updated = new \DateTime();
    }

    public function hydrator(array $options) : Role
    {
        $hydrator = new Hydrator\ClassMethods();
        $hydrator->hydrate($options, $this);
        return $this;
    }

    public function toArray() : array
    {
        $hydrator = new Hydrator\ClassMethods();
        return $hydrator->extract($this);
    }
}
```

### - Snippet Entity
```php
namespace App\Entity;

use Core\Entity\AbstractEntity;
use Doctrine\ORM\Mapping as ORM;

/**
 * @author Jhon Mike <developer@jhonmike.com.br>
 *
 * @ORM\Entity
 * @ORM\Table(name="role")
 */
class Role extends AbstractEntity
{
    /**
     * @ORM\Column(type="string", length=100, nullable=false)
     */
    private $name;

    public function setName(string $name) : Role
    {
        $this->name = $name;
        return $this;
    }

    public function getName() : string
    {
        return $this->name;
    }
}
```

### - Snippet Entity Clean
```php
namespace App\Entity;

use DateTime;
use Doctrine\ORM\Mapping as ORM;
use Zend\Hydrator;
use Zend\Math\Rand;

/**
 * @author Jhon Mike <developer@jhonmike.com.br>
 *
 * @ORM\Entity
 * @ORM\Table(name="role")
 * @ORM\HasLifecycleCallbacks
 */
class Role
{
    /**
     * @ORM\Id
     * @ORM\Column(type="integer", nullable=false)
     * @ORM\GeneratedValue(strategy="IDENTITY")
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=100, nullable=false)
     */
    private $name;

    /**
     * @ORM\Column(type="datetime", nullable=false)
     */
    private $created;

    /**
     * @ORM\Column(type="datetime", nullable=false)
     */
    private $updated;

    public function getId() : int
    {
        return $this->id;
    }

    public function setName(string $name) : Role
    {
        $this->name = $name;
        return $this;
    }

    public function getName() : string
    {
        return $this->name;
    }

    public function getCreated() : DateTime
    {
        return $this->created;
    }

    /**
     * @ORM\PrePersist
     */
    public function preCreated()
    {
        $this->created = new \DateTime();
    }


    public function getUpdated() : DateTime
    {
        return $this->updated;
    }

    /**
     * @ORM\PrePersist
     * @ORM\PreUpdate
     */
    public function preUpdated()
    {
        $this->updated = new \DateTime();
    }

    public function hydrator(array $options) : Role
    {
        $hydrator = new Hydrator\ClassMethods();
        $hydrator->hydrate($options, $this);
        return $this;
    }

    public function toArray() : array
    {
        $hydrator = new Hydrator\ClassMethods();
        return $hydrator->extract($this);
    }
}
```
