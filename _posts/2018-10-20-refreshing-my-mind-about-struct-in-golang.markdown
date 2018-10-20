---
layout: post
title: "using superheroes for refresh my mind about Golang structs"
date: 2018-10-20 15:58:38 +0200
categories: golang
---

After have followed [an amazing course about golang](https://greatercommons.com/learn/golang) one years ago, I had need to refresh my mind about the [Struct](https://golang.org/ref/spec) in golang, and for do it I had used some superheroes:

### First Example:
```
package main

import "fmt"

type skill struct {
	Power string
}

type hero struct {
	Name  string
	Alias string
	skill
}

func main() {
	h1 := hero{
		Name:  "Bruce Wayne",
		Alias: "Batman",
		skill: skill{
			Power: "a lot of gadgets",
		},
	}
	h2 := hero{
		Name:  "Tony Stark",
		Alias: "Iron",
		skill: skill{
			Power: "hi-tech armor",
		},
	}

	fmt.Printf("the best skill of %s is %s \n", h1.Alias, h1.skill.Power)
	fmt.Printf("the best skill of %s is %s \n", h2.Alias, h2.skill.Power)

	s1 := skill{
		Power: "velocity",
	}

	h3 := hero{
		Name:  "Barry Allen",
		Alias: "Flash",
		skill: skill{
			Power: s1.Power,
		},
	}

	fmt.Printf("the best skill of %s is %s \n", h3.Alias, h3.skill.Power)
}
```

### Second Example
```
package main

import "fmt"

type skill struct {
	Power string
}

type hero struct {
	Name  string
	Alias string
}

type heroSkill struct {
	hero
	skill
}

func main() {
	hs1 := heroSkill{
		hero: hero{
			Name:  "Clark Kent / Kal-El",
			Alias: "Superman",
		},
		skill: skill{
			Power: "a lot of cool superpowers",
		},
	}

	hs2 := heroSkill{
		hero: hero{
			Name:  "Steve Rogers",
			Alias: "Capitan America",
		},
		skill: skill{
			Power: "strong",
		},
	}

	fmt.Printf("the best skill of %s is %s \n", hs1.hero.Alias, hs1.skill.Power)
	// check the different way to call the struct, in this second Print, there no need to call the sub struct.
	fmt.Printf("the best skill of %s is %s \n", hs2.Alias, hs2.Power)
}

```