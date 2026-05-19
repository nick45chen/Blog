---
title: '[Android] Job scheduler'
date: 2017-10-21 16:03:33
tags: 'Android'
---

## Introduction

Job Scheduler 是一種高效的背景作業，能在一定程度上提高電池的使用壽命，同時也能給開發者節省開發的時間。
<!-- more --> 
在Android 開發中，你可能會遇到，需要在某個時間點或滿足某個條件時，才執行特定的任務。
例如：當設備連上Wi-Fi或接上電源的時候，或是每過15分鐘，定期上傳數據資料，在以前我們可能考慮過使用 Handlers 延遲上傳，但萬一用戶或系統關閉他們的應用程式呢？這時你可能會考慮使用AlarmManager ，前提是在用戶沒有關機過的狀態下，才能正常運行。另外，我們還沒把網路狀態改變的情況(斷線)考慮進去，這時你可能需要註冊一個廣播，在網路恢復時進行額外的邏輯處理，以確保你的程序正常的執行。


以上種種情形，真的是會讓開發者苦惱許久，幸運的是現在Google都幫你準備好了，Job Scheduler已經與新的系統緊密的結合，你只需要事先設定好執行的條件，條件的判斷就交給系統去處理，這個新的 API 只會在你滿足條件時執行。

看看我們實際上如何做到這一點

### 1.創建Job Service

``` bash
public class JobSchedulerService extends JobService {
    @Override
    public boolean onStartJob(JobParameters params) {
    	//返回true，表示該工作耗時，需要執行結束時手動調用jobFinished()，來銷毀任務
        //返回false，代表任務處理不需要很長的時間，在 return 時已經處理結束
        return false;
    }

    @Override
    public boolean onStopJob(JobParameters params) {
    	//只有在 onStartJob() 返回 true 時，手動調用 jobFinished() 後才會執行此方法
    	//返回false來銷毀此工作
        return false;
    }
}
```

### 2.創建一個JobScheduler對象

``` bash
//job id 用以區別任務
int JOB_ID = 0
JobInfo.Builder jobBuilder = JobInfo.Builder(JOB_ID, new ComponentName(getPackageName(), JobSchedulerService.class.getName()));
```
##### JobInfo.Builder 還允許你設定不同的條件，任務只會在符合設置條件時執行

``` bash
//設置任務延遲執行的時間，單位毫秒
jobBuilder.setMinimumLatency(long minLatencyMillis)

//設置任務最後延遲時間，如果時間到了其他條件還未滿足，任務也會啟動
 jobBuilder.setOverrideDeadline(long maxExecutionDelayMillis)

//設置在有網路連線的狀態下執行，預設是NETWORK_TYPE_NONE，意即不需要網路
jobBuilder.setRequiredNetworkType(int networkType);

//是否在充電時執行，並非充電馬上觸發，只有在電量健康的時候(>15%)，才會觸發
//jobBuilder.setRequiresCharging(boolean requiresCharging)

//設置是否在設備重啟後，要繼續執行
jobBuilder.setPersisted(boolean isPersisted)

//設置任務運行的週期，與setMinimumLatency()和setOverrideDeadline()不兼容，同時設置會引發異常
jobBuilder.setPeriodic(long time)

//是否在空閒的時候執行，只有當用戶一段時間沒有使用設備時，才會觸發
jobBuilder.setRequiresDeviceIdle(boolean requiresDeviceIdle)
```

### 3.構建一個JobIndo對象，並將它發送到JobScheduler中

``` bash
JobScheduler mJobScheduler = (JobScheduler)getSystemService(Context.JOB_SCHEDULER_SERVICE);
mJobScheduler.schedule(jobBuilder.build())
```
##### 在JobScheduler.schedule(JobInfo jobInfo)中，會返回一個int值，代表此任務是否安排成功，如果返回值小於0，代表任務失敗，反之則會返回該任務的id
``` bash
 if ((mJobScheduler.schedule(jobBuilder.build())) <= 0) {
    Log.i(TAG, " something goes wrong")
 }
```

參考網站：
[Android JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)
[Android Intelligent Job-Scheduling](https://developer.android.com/topic/performance/scheduling.html)
[Youtube Android Developer](https://www.youtube.com/watch?v=QdINLG5QrJc&t=252s)





