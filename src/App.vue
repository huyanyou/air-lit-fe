<script setup lang="ts">
const { darkMode, statusBarHeight, menuButtonBounding } = storeToRefs(useAppStore());
const { setDarkMode } = useAppStore();

onLaunch(() => {
  // #ifdef MP-WEIXIN || MP-QQ
  const systemInfo = uni.getSystemInfoSync();
  // the systemInfo.theme is only support dark mode in WeChat and QQ
  darkMode.value = systemInfo?.theme === "dark";
  statusBarHeight.value = systemInfo!.statusBarHeight || 44;
  menuButtonBounding.value = uni.getMenuButtonBoundingClientRect();
  uni.onThemeChange((res: UniApp.OnThemeChangeCallbackResult) =>
    setDarkMode(res.theme === "dark")
  );
  // #endif

  // #ifdef H5
  const colorScheme = window.matchMedia("(prefers-color-scheme: dark)");
  darkMode.value = colorScheme.matches;
  colorScheme.addEventListener("change", (e: MediaQueryListEvent) =>
    setDarkMode(e.matches)
  );
  // The data is obtained from iPhone13 miniprogram but statusBarHeight, top and bottom values are subtracted from the statusBarHeight value
  statusBarHeight.value = 0;
  menuButtonBounding.value = {
    width: 87,
    height: 32,
    left: 281,
    top: 4,
    right: 368,
    bottom: 36,
  };
  // #endif
});
onShow(() => {});
onHide(() => {});
</script>
