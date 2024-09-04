## Fork debug

### u_int32_t

Fix

```cpp
#include <stdint.h>

using uint = std::uint32_t;
```

### API version

Bug

```
ERROR:             VkInstanceCreateInfo::pApplicationInfo::apiVersion has value of 0 which is not permitted. If apiVersion is not 0, then it must be greater than or equal to the value of VK_API_VERSION_1_0 [VUID-VkApplicationInfo-apiVersion]
```

Fix

src\VulkanContext.cpp

```cpp
const std::vector<const char *> validation_layers{"VK_LAYER_KHRONOS_validation"};
vk::ApplicationInfo app_info{};
app_info.sType = vk::StructureType::eApplicationInfo;
app_info.pApplicationName = "Hello Vulkan";
app_info.applicationVersion = VK_MAKE_VERSION(1, 0, 0);
app_info.pEngineName = "No Engine";
app_info.engineVersion = VK_MAKE_VERSION(1, 0, 0);
app_info.apiVersion = VK_API_VERSION_1_0;

Instance = vk::createInstanceUnique({flags, &app_info, validation_layers, extensions});
```

### device extension

Bug

```
validation layer: loader_validate_device_extensions: Device extension VK_KHR_portability_subset not supported by selected physical device or enabled layers.
validation layer: vkCreateDevice: Failed to validate extensions in list
```

Fix

src\VulkanContext.cpp

```cpp
const std::vector<const char *> device_extensions{VK_KHR_SWAPCHAIN_EXTENSION_NAME};
```