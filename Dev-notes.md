### Adding new Affixes

files needed : 
**AffixesParser.scala ComputedItem.scala ComputedItemProps.scala ProperItems.scala**
AffixesParser.scala       - choosing to what group affix belong,  how ingame affix is worded, and deciding if it needs to be sumed/substracted/multiplied/divided by some other stat, or just how program is going to recognize this affix. Code excerpt

```
def increased(name: String)(f: (ComputedItem, Double) => Unit) = { regex1(s"^([.+-\\d]+)%* increased $name$$")(f) }
  def reduced(name: String)(f: (ComputedItem, Double) => Unit) = { regex1(s"^([.+-\\d]+)%* reduced $name$$")(f) }
  def plusTo(name: String)(f: (ComputedItem, Double) => Unit) = { regex1(s"^([.+-\\d]+)%* to $name$$")(f) }
  def addsDamage(element: String, suffix : String = "")(f: (ComputedItem, Double, Double) => Unit) = {
    regex2(s"^Adds ([\\d]+) to ([\\d]+) $element Damage${suffix}$$")(f)
  }
  def level(name: String)(f: (ComputedItem, Double) => Unit) = {
    val a = if (name.isEmpty) "" else name + " "
    val r = s"^([.+-\\d]+)%* to Level of Socketed ${a}Gems$$"
    regex1(r)(f)
  }
  def simple1(prefix: String, suffix: String)(f: (ComputedItem, Double) => Unit) = {
    val a = if (prefix.isEmpty) "" else prefix + " "
    val b = if (suffix.isEmpty) "" else " " + suffix
    val r = s"^$a([.+-\\d]+)%*$b$$"
    regex1(r)(f)
```


ComputedItem.scala        - adding affix as some variable to designed section/type. Code excerpt

```
  object leech {var physical = LifeAndMana mutable 0.0}
  object onKill {var lifeAndMana = LifeAndMana mutable 0.0}
  object onAttackHit {var lifeAndMana = LifeAndMana mutable 0.0}
  object gemLevel {

  object total {
  ...
    def armour = properties.armour.oIf(_ == 0.0, x => plusTo.armour, x => x)
    def evasionRating = properties.evasionRating.oIf(_ == 0.0, x => plusTo.evasionRating, x => x)
    def energyShield = properties.energyShield.oIf(_ == 0.0, x => plusTo.energyShield, x => x)
    def globalEnergyShield = increased.maximumEnergyShield + plusTo.attribute.intelligence * .2
    def critChance = (100 + increased.globalCriticalStrikeChance) / 100.0 *
      properties.criticalStrikeChance
  }

  object slots {
  }


  object properties {
    var weaponType: WeaponType = WeaponTypes.NoWeaponType
    var armour                 = 0.0
    var energyShield           = 0.0
    var evasionRating          = 0.0
    val damages                = Elements of MinMaxDamage(0, 0)
    var quality                = 0.0
    var criticalStrikeChance   = 0.0
    var attacksPerSecond       = 0.0
    var chanceToBlock          = 0.0
    var weaponRange          = 0.0
    var stackSize              = 0.0
  }

```


ComputedItemProps.scala   - front end interface - how affix will be named as choosable option in Select Column Panel, and to what group of mods it will be assigned

```
//Efficiency
  val MagicFind                = pno("MagicFind", "mf")(Efficiency)(_.magicFind)
  val IncreasedItemRarity      = pno("IncreasedItemRarity", "iir")(Efficiency)(_.increased.rarityOfItemsFound)
  val IncreasedItemQuantity    = pno("IncreasedItemQuantity", "iiq")(Efficiency)(_.increased.quantityOfItemsFound)
  val IncreasedMoveSpeed       = pno("IncreasedMoveSpeed", "+%move")(Efficiency)(_.increased.movementSpeed)
  val IncreasedProjectileSpeed = pno("IncreasedProjectileSpeed", "+%projSpeed")(Efficiency)(_.increased.projectileSpeed)
  MagicFind ?= "Total of Increased Item Rarity and Increased Item Quantity"
  IncreasedItemRarity !?= "Increased Rarity of Items Found"
  IncreasedItemQuantity !?= "Increased Quantity of Items Found"
  IncreasedMoveSpeed ?= "Increased Movement Speed"
  IncreasedProjectileSpeed !?= "Increased Projectile Speed"

  //Regen
  val LifeLeech          = pno("LifeLeech", "lleech")(Regen)(_.leech.physical.life)
  val LifeGainOnHit      = pno("LifeGainOnHit", "lgoh")(Regen)(_.onAttackHit.lifeAndMana.life)
  val ManaGainOnHit      = pno("ManaGainOnHit", "mgoh")(Regen)(_.onAttackHit.lifeAndMana.mana)
  val LifeGainOnKill     = pno("LifeGainOnKill", "lgok")(Regen)(_.onKill.lifeAndMana.life)
  val ManaGainOnKill     = pno("ManaGainOnKill", "mgok")(Regen)(_.onKill.lifeAndMana.mana)
  val ManaLeech          = pno("ManaLeech", "mleech")(Regen)(_.leech.physical.mana)
  val LifeRegen          = pno("LifeRegen", "lireg")(Regen)(_.regeneratedPerSecond.life)
  val IncreasedManaRegen = pno("IncreasedManaRegen", "+%mareg")(Regen)(_.increased.manaRegenerationRate)
  val MinusToManaCostOfSkills = pno("MinusToManaCostOfSkills", "-mcos")(Regen)(_.minusToManaCostOfSkills)
  LifeLeech !?= "Life Leech from Physical Attack Damage"
  LifeGainOnHit !?= "Life Gain on Hit"
  ManaGainOnHit !?= "Mana Gain on Hit"
  LifeGainOnKill !?= "Life Gain on Kill"
  ManaGainOnKill !?= "Mana Gain on Kill"
  ManaLeech !?= "Mana Leech from Physical Attack Damage"
  LifeRegen !?= "Life Regeneration Rate"
  IncreasedManaRegen !?= "Increased Mana Regeneration Rate"
  MinusToManaCostOfSkills !?= "Mana Cost of Skills"
```

ProperItems.scala - affix named in ComputedItemProps.scala  for example val = movementSpeed is filled with value parsed from JSON with val movementSpeed = p1()

```
object increased {
    val movementSpeed = p1()


    val manaRegenerationRate = p1()

    val quantityOfItemsFound = p1()
    val rarityOfItemsFound   = p1()
```
congratulation, new affix is being parsed!