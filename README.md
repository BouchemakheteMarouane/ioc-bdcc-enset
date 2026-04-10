# Activité Pratique N°1 - Injection des Dépendances

**Réalisé par :** Bouchemakhete Marouane  
**Module :** Frameworks Java - Spring IoC  
**Encadrant :** Mohamed YOUSSFI

---

## Description

Ce projet illustre le concept d'**Injection des Dépendances (IoC)** en Java, en utilisant différentes approches allant de l'instanciation manuelle jusqu'au Framework Spring.

---

## Architecture du projet

```
src/main/java/
├── dao/
│   ├── IDao.java              # Interface DAO
│   └── DaoImpl.java           # Implémentation DAO
├── metier/
│   ├── IMetier.java           # Interface Métier
│   └── MetierImpl.java        # Implémentation Métier (couplage faible)
├── ext/
│   └── DaoImplV2.java         # Autre implémentation DAO
├── net.Rafiky/
└── pres/
    ├── Pres.java                    # Injection statique & dynamique
    ├── PresAvecSpringXml.java        # Injection via Spring XML
    └── PressSpringAnnotation.java    # Injection via Annotations Spring
src/main/resources/
└── config.xml                 # Configuration Spring XML
```

---

## Partie 1 : Implémentation

### 1. Interface IDao
Définit la méthode `getData()` qui retourne une valeur de type `double`.

### 2. Implémentation DaoImpl
Implémente `IDao` et retourne une donnée simulée.

### 3. Interface IMetier
Définit la méthode `calcul()` qui effectue un traitement métier.

### 4. Implémentation MetierImpl
Implémente `IMetier` avec un **couplage faible** via l'interface `IDao`.

---

## Partie 2 : Injection des Dépendances

### a. Instanciation Statique
```java
DaoImpl dao = new DaoImpl();
MetierImpl metier = new MetierImpl();
metier.setDao(dao);
System.out.println(metier.calcul());
```

### b. Instanciation Dynamique
```java
String daoClassName = "dao.DaoImpl";
Class c = Class.forName(daoClassName);
IDao dao = (IDao) c.getConstructor().newInstance();
IMetier metier = new MetierImpl();
metier.setDao(dao);
System.out.println(metier.calcul());
```

### c. Spring Framework - Version XML
Configuration dans `config.xml` :
```xml
<bean id="dao" class="dao.DaoImpl"/>
<bean id="metier" class="metier.MetierImpl">
    <property name="dao" ref="dao"/>
</bean>
```
```java
ApplicationContext context = new ClassPathXmlApplicationContext("config.xml");
IMetier metier = context.getBean(IMetier.class);
System.out.println(metier.calcul());
```

### d. Spring Framework - Version Annotations
```java
@Component("dao")
public class DaoImpl implements IDao { ... }

@Component("metier")
public class MetierImpl implements IMetier {
    @Autowired
    private IDao dao;
}
```

---

## Résultat

Toutes les approches retournent le même résultat : **576.0**

---

## Technologies utilisées

- Java 17
- Spring Framework 6.2.17
- Maven
- IntelliJ IDEA

---

## Lancer le projet

```cmd
cd ioc-bdcc-enset
mvnw.cmd compile
```
Puis lancer la classe souhaitée depuis IntelliJ :
- `Pres.java` → instanciation statique/dynamique
- `PresAvecSpringXml.java` → version XML
- `PressSpringAnnotation.java` → version annotations
