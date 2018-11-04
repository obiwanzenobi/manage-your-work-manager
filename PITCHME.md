
# Manage your workmanager
![ninja](/images/miniature-figure-1745718_1280.jpg)
### How to use the new WorkManager like a boss
---
## Work manager
- Gwarantuje wykonanie zleconych czynności w ściśle określonych warunkach |
- Przestrzega ograniczeń wykonywania zadań w tle (doze mode itd.) |
- Kompatybilne wstecznie także bez Google Play services |
- Możliwość wstrzykiwania zależności przez specjalną fabryke |
- Opcja sprawdzenia stanu danego zlecenia |
- Zlecenia okresowe oraz pojedyncze |
+++
## Quickstart
```kotlin
val workManager = WorkManager.getInstance()

val workConstraints = Constraints.Builder()
   .setRequiredNetworkType(NetworkType.CONNECTED)
   .setRequiresBatteryNotLow(true)
// .setRequiresCharging()
// .setRequiresDeviceIdle()
// .setRequiresStorageNotLow()
   .build() 
val workRequest = OneTimeWorkRequestBuilder<SendPhotoWorker>()
   .setConstraints(workConstraints)
   .setInputData(createSendPhotoRequest(file.path))
   .build()

workManager.enqueue(workRequest)
```

+++
## Quickstart Worker
```kotlin
class SendPhotoWorker(...) : Worker(...) {
    private val filePath by lazy {
        inputData.getString(PHOTO_PATH_KEY)
    }

    override fun doWork(): Result {
        val pathUri = Uri.fromFile(File(filePath))
        val storage = FirebaseStorage.getInstance().reference
        val stream = FileInputStream(File(filePath))
        val uploadTask = storage.child("images/" + pathUri.lastPathSegment).putStream(stream)
        Tasks.await(uploadTask)

        return if (uploadTask.isSuccessful) Result.SUCCESS else Result.RETRY
    }
}

fun createSendPhotoRequest(photoPath: String) = workDataOf(PHOTO_PATH_KEY to photoPath)
```
---
## Ustawienie ilości ponowień
- Brak cienia |
- Cień nie jest rysowany w całości |
- Widok jest niewidoczny lub widoczny częściowo |
- Brak animacji cieni |
- Cienie, nie będące elevation (np. bitmapy) |

+++?image=images/shadowsTranslation.png

## Jak elevation jest rysowane?
- View.java |
- RenderNode.java -> RenderNode.cpp |
- FrameBuilder.cpp |
- TesselationCache.cpp |
- ShadowTessellator.cpp -> Ambient/SpotShadow.cpp |

---
## Elevation, dropshadow czy image?
+++
```
adb shell dumpsys gfxinfo packageName

./systrace.py -a packageName
```
@[1](Informacje GPU)
@[3](Systrace dla danego pakietu)

---
### Questions?

<br>

@fa[twitter gp-contact](@wojciech_warwas)

@fa[github gp-contact](@obiwanzenobi)
