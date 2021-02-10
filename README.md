# Automatic Reference Counting

ARC

Какие списки захвата в Swift: 
-> weak, strong и unowned
в чём разница между ссылками weak, strong и unowned?

«Сильный» захват (strong capturing)
 -> До тех пор, пока вы явно не указываете способ захвата, Swift использует «сильный» захват.

```swift
class Singer {
    func playSong() {
        print("Shake it off!")
    }
}

func sing() -> () -> Void {
    let taylor = Singer()
    
    let singing = {
        taylor.playSong()
        return
    }
    
    return singing
}
```


«Слабый» захват (weak capturing)
	-> Swift позволяет нам создать "список захвата", чтобы определить, каким именно образом захватываются используемые значения.
	1. «Слабо» захваченные значения не удерживаются замыканием и, таким образом, они могут быть освобождены и установлены в nil.

	2. Как следствие первого пункта, «слабо» захваченные значения в Swift всегда optional. 
Мы модифицируем наш пример с использованием «слабого» захвата и сразу же увидим разницу.

// will print Shake it off!
```swift
class Singer {
    func playSong() {
        print("Shake it off!")
    }
    
    func sing() -> () -> Void {
        let singing = { [weak self] in 
            self?.playSong()
            return
        }
        
        return singing
    }
}

let taylor = Singer()
let singFunction = taylor.sing()
singFunction()
```

«Бесхозный» захват (unowned capturing)

// this code will crash 
```swift
class Singer {
    func playSong() {
        print("Shake it off!")
    }
}

let taylor = Singer()

func sing() -> () -> Void {
       
    let singing = { [unowned taylor] in
        taylor.playSong()
        return
    }
    
    return singing
}

// this will run and print Shake it off!
class Singer {
    func playSong() {
        print("Shake it off!")
    }
}

let taylor = Singer()

func sing() -> () -> Void {
    
    let singing = { [unowned taylor] in 
        taylor.playSong()
        return
    }
    
    return singing
}
```

———
