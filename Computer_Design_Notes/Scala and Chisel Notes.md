


## Scala/Chisel Examples



### Conditional addition / subtraction with a multiplexer



```scala
class ScalaConditional(opSel: Boolean) extends Module {
  val io = IO(
    new Bundle {
      val a = Input(UInt(32.W))
      val b = Input(UInt(32.W))

      val out = Output(UInt(32.W))
    }
  )

  if(opSel){
    io.out := io.a + io.b
  } else {
    io.out := io.a - io.b
  }
}
```



![[Screenshot 2026-08-27 at 12.33.24.png]]


###  Programmatically assembling modules

```Scala
class MyIncrementN(val incrementBy: Int, val numIncrementors: Int) extends Module {
  val io = IO(
    new Bundle {
      val dataIn  = Input(UInt(32.W))
      val dataOut = Output(UInt(32.W))
    }
  )

  // Each module is stored in an array. Arrays are a scala construct, which means
  // they can only be accessed with a scala int.
  val incrementors = Array.fill(numIncrementors){ Module(new MyIncrement(incrementBy)) }

  // the data input is connected to the previous modules output, creating what is known as
  // a "human centipede" in popular culture.
  for(ii <- 1 until numIncrementors){
    incrementors(ii).io.dataIn := incrementors(ii - 1).io.dataOut
  }

  incrementors(0).io.dataIn := io.dataIn
  io.dataOut := incrementors.last.io.dataOut
}
```

### Indexing collection of elements

```Scala
class MyVector() extends Module {
  val io = IO(
    new Bundle {
      val idx = Input(UInt(32.W))
      val out = Output(UInt(32.W))
    }
  )

  val values = Vec(1.U, 2.U, 3.U, 4.U)
  
  io.out := values(io.idx)
}
```

### Stateful Circuits

This circuit stores its input in delayReg and drives its output with delayRegs output. Registers are driven by a clock signal in addition to the input value, and it is only capable of updating its value at a clock pulse.

```scala
class SimpleDelay() extends Module {
  val io = IO(
    new Bundle {
      val dataIn  = Input(UInt(32.W))
      val dataOut = Output(UInt(32.W))
    }
  )
  val delayReg = RegInit(UInt(32.W), 0.U)

  delayReg   := io.dataIn
  io.dataOut := delayReg
}
```


### Printf

```Scala
class PrintfExample() extends Module {
  val io = IO(new Bundle{})
  
  val counter = RegInit(0.U(8.W))
  counter := counter + 1.U

  printf("Counter is %d\n", counter)
  when(counter % 2.U === 0.U){
    printf("Counter is even\n")
  }
}

class PrintfTest(c: PrintfExample) extends PeekPokeTester(c)  {
  for(ii <- 0 until 5){
    println(s"At cycle $ii:")
    step(1)
  }
}
```
