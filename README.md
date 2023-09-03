# DrainAwareTransform

Backpressure-aware stream.Transform class

Well-behaved node.js Readable streams
[should](https://nodejs.org/en/docs/guides/backpressuring-in-streams)
respect the Stream API and stop pushing data when push() returns false.
The problem is that there is no built-in way for a Readable to know when
its internal buffer is drained enough to resume pushing.

This class extends stream.Transform with an event 'continue' that is
analogous to the 'drain' event of stream.Writable. When push()
returns false, the transform waits until its buffer is drained and
the _read() private method is called again. Then 'continue' is
emitted and the transform can continue pushing data.

A convenience method 'safePush' is also provided. 'safePush' pushes
data and then returns a Promise that resolves when it is safe to push
again.

```
abstract class DrainAwareTransform extends stream.Transform {
  protected safePush(chunk: any, encoding?: BufferEncoding): Promise<void>;
}
```

## Getting Started

```
npm install 'drain-aware-transform'
```

```
import DrainAwareTransform from 'drain-aware-transform';

class MyTransform extends DrainAwareTransform {

  _transform(chunk, encoding, callback) {
    // your implementation
  }

  _flush(callback) {
    // your implementation
  }
}
