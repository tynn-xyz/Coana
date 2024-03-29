# Coana
[![Build][build-shield]][build]
[![Download][download-shield]][download]

#### _Coroutine Analytics_ `MetaData` elements

_Coana_ provides a simple model to attach and access properties for analysis purpose.


## Installation

    implementation "xyz.tynn.coana:library:$coanaVersion"


## Usage

The `Coana` model contains a scope and a context to distinguish between
different scenarios. Additional to this it contains `Double`, `Long` and
`String` properties mapped to a set of predefined keys. It is accessible with
the `CoroutineScope.coana` and `CoroutineContext.coana` extensions.

Every single property is a `CoroutineContext.Element` by itself and is
contained within the surrounding context. Adding another element with the same
key will override the element while the original is contained within the
original context.

### `CoanaScope` and `CoanaContext`

    val coanaScope = CoanaScope("scope")
    val coanaContext = CoanaContext("context")

### `CoanaPropertyKey`

    sealed class PropertyKey<Value> : CoanaPropertyKey<Value> {
        object ApiVersion : PropertyKey<Double>
        object AppVersion : PropertyKey<Long>
        object Dependency : PropertyKey<String>
    }

### `CoanaProperty`

    val coanaDouble = CoanaProperty(ApiVersion, 1.2)
    val coanaLong = CoanaProperty(AppVersion, 3)
    val coanaString = CoanaProperty(Dependency, "4.5.6")

### `Coana`

    assert(coana.scope == "scope")
    assert(coana.context == "context")
    assert(coana.doubleProperties[ApiVersion] == 1.2)
    assert(coana.longProperties[AppVersion] == 3)
    assert(coana.stringProperties[Dependency] == "4.5.6")

### Coroutine _DSL_

    val coana = withCoanaScope("scope") {
        withCoanaContext("context) {
            withCoanaProperty(ApiVersion, 1.2) {
                withContext(coanaLong + coanaString) {
                    coana
                }
            }
        }
    }


## MetaData

The `MetaData` provides a generic implementation of a _key-value_ pair attached
to a `CoroutineContext`.

    object StringKey : Key<String>

    val meta = MetaData(StringKey, "stringValue")

    val data = withContext(meta) {
        coroutineContext[StringKey]
    }

    assert(meta == data)

Use `CoroutineContext.metadata` to get all a set of all `MetaData` attached
to the context.

### Installation

    implementation "xyz.tynn.coana:metadata:$coanaVersion"


## License

    Copyright (C) 2019 Christian Schmitz

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


  [build]: https://github.com/tynn-xyz/Coana/actions
  [build-shield]: https://img.shields.io/github/workflow/status/tynn-xyz/Coana/Build
  [download]: https://search.maven.org/search?q=xyz.tynn.coana
  [download-shield]: https://img.shields.io/maven-central/v/xyz.tynn.coana/library
