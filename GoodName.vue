<template>
  <div class="good">
    <p>Hello Manvi, how are you?</p>

    <!-- Button to trigger download -->
    <button @click="showImage" class="download-btn">Download</button>

    <!-- Image that appears on button click -->
    <img 
      v-if="isImageVisible" 
      src="@/assets/download.png" 
      alt="Download Image" 
      class="download-image" 
      @click="toggleTray" 
    />

    <!-- Badge to show number of ongoing downloads -->
  <span v-if="ongoingDownloads.length > 0" class="download-count-badge">
    {{ ongoingDownloads.length }}
  </span>

    <!-- Tray that appears below the image when the image is clicked -->
    <div v-if="isTrayVisible" class="tray">
      <div class="tray-header">
        <h3>Recent Download History</h3>
        <img src="@/assets/cancel2.png" alt="Cancel" class="cancel-icon" @click="closeTray" />
      </div>

      <!-- Loading progress for ongoing downloads -->
      <section class="loading-area">
        <li v-for="(file, index) in ongoingDownloads" :key="index" class="row">
          <img src="@/assets/file.png" alt="File Icon" class="file-icon" />
          <div class="content">
            <div class="details">
              <span class="name">{{ file.name }}</span>
              <span class="percent">{{ file.progress }}%</span>
            </div>
            <div class="loading-bar">
              <div class="loading" :style="{ width: file.progress + '%' }"></div>
            </div>
          </div>
        </li>
      </section>

      <!-- Completed downloads list -->
      <section class="download-area">
        <li v-for="(file, index) in completedDownloads" :key="index" class="row">
          <div class="content download">
            <img src="@/assets/file.png" alt="File Icon" class="file-icon" />
            <div class="details">
              <span class="name">{{ file.name }}</span>
              <span class="size">{{ file.size }}</span>
            </div>
          </div>
          <img src="@/assets/done.png" alt="Done Icon" class="file-icon" />
        </li>
      </section>
    </div>
  </div>
</template>

  







<script>
export default {
  name: 'GoodNews',
  data() {
    return {
      isImageVisible: false,      // Track image visibility
      isTrayVisible: false,       // Track tray visibility
      ongoingDownloads: [],       // List for ongoing downloads
      completedDownloads: []      // List for completed downloads
    };
  },
  methods: {
    showImage() {
      this.isImageVisible = true;  // Show the image on button click
      this.startDownload();        // Trigger download simulation
    },
    toggleTray() {
      this.isTrayVisible = !this.isTrayVisible;  // Toggle the tray visibility
    },
    closeTray() {
      this.isTrayVisible = false;  // Close the tray when cancel icon is clicked
    },
    
  startDownload() {
    // Simulate a download for 3 files
    const files = [
      { name: 'File1', size: '2MB', progress: 0 },
      { name: 'File2', size: '4MB', progress: 0 },
      { name: 'File3', size: '1MB', progress: 0 }
    ];

    // Push files into ongoing downloads
    this.ongoingDownloads = [...files];  // Spread the array to avoid reference issues

    files.forEach((file, fileIndex) => {
      let progress = 0;

      // Simulate download progress
      const interval = setInterval(() => {
        if (progress < 100) {
          progress += 10;
          this.ongoingDownloads[fileIndex].progress = progress;
        } else {
          // Once download is complete, move file to completed downloads
          this.completedDownloads.push({
            name: file.name,
            size: file.size
          });

          // Clear the interval for this file
          clearInterval(interval);

          // Remove file from ongoing downloads AFTER clearing interval
          // Use filter to avoid index-related issues
          this.ongoingDownloads = this.ongoingDownloads.filter(
            (f) => f.name !== file.name
          );
        }
      }, 500);  // Adjust the speed of the progress simulation
    });
  
}

  }
};
</script>




  







  <style scoped>
  .good {
    position: relative;
  }
  
  .download-btn {
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #015685;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
  }
  
  .download-image {
    position: fixed; 
    top: 10px; 
    right: 10px;  
    width: 45px;  
    height: auto; 
    z-index: 1; 
    cursor: pointer;
  }
  .download-wrapper {
  position: relative; /* To position the badge relative to the image */
  }
  .download-count-badge {
  position: absolute;
  top: -22px;  /* Move the badge 10px higher */
  right: -4px;
  background-color: #ee8001;
  color: white;
  border-radius: 50%;
  width: 23px;
  height: 23px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  font-weight: bold;
  z-index: 2;
}

  .tray {
    position: fixed;
    top: 50px;  
    right: 50px;  
    width: 250px;
    margin-left: 20px;
    background-color: white;
    border: 1px solid #ccc;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    padding: 10px;
    z-index: 999; 
    text-align: left;
  }
  
  .tray-header {
    font-size: smaller;
    margin-left: 20;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .cancel-icon {
    width: 22px;  /* Adjust size of the cancel icon */
    height: auto;
    cursor: pointer;
  }
  
  .download-btn:hover {
    background-color: #b1d2e3;
  }
  section .row{
    margin-bottom: 5px;
    background-color: #c6e1f0;
    list-style: none;
    padding: 10px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  section .row{
    color: #015685;
    font-size: 30px;
  }
  section .details span{
    font-size:12px;
  }

  .loading-area .row .content{
    width: 100%;
    margin-left: 15px;
  }
  .loading-area .details{
    display: flex;
    align-items:center;
    margin-bottom: 7px;
    justify-content: space-between;
  }
  .loading-area .content .loading-bar{
    height: 6px;
    width: 100%;
    margin-bottom: 4px;
    background: #fff;
    border-radius: 30px;
  }
  .loading{
    height: 100%;
    width: 20%;
    background: #015685;
    border-radius: inherit;
  }
  .download-area{
    max-height: 232px;
    overflow-y: scroll;
  }
  .download-area .onprogress{
    max-height: 150px;
  }
  .download-area::-webkit-scrollbar{
    width:0px;
  }
  .download-area .row .content{
    display: flex;
    align-items: center;
  }
  .download-area .row .details{
    display: flex;
    margin-left: 15px;
    flex-direction: column;
  }
  .download-area .row .details .size {
    columns: #404040;
    font-size: 11px;
  }
  .file-icon{
    weight:20px;
    height: 27px;
  }

  

  </style>
  