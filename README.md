<!doctype html>
<html lang="vi">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Ôn Tập Pháp Luật Đại Cương + AI</title>

    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>

    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <!-- Google Fonts -->
    <link
      href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;800&display=swap"
      rel="stylesheet"
    />

    <!-- Markdown Parser (Marked) for AI responses -->
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>

    <!-- Chosen Palette: Warm Neutrals with Amber Accents -->

    <style>
      body {
        font-family: "Inter", sans-serif;
        background-color: #fdfbf7; /* Warm neutral background */
        color: #422006; /* Dark brown text for contrast */
      }

      .chart-container {
        position: relative;
        width: 100%;
        max-width: 800px;
        margin-left: auto;
        margin-right: auto;
        height: 300px;
        max-height: 400px;
      }

      /* Custom Scrollbar */
      ::-webkit-scrollbar {
        width: 8px;
      }
      ::-webkit-scrollbar-track {
        background: #f1f1f1;
      }
      ::-webkit-scrollbar-thumb {
        background: #d97706;
        border-radius: 4px;
      }
      ::-webkit-scrollbar-thumb:hover {
        background: #b45309;
      }

      .btn-option {
        transition: all 0.2s ease;
      }
      .btn-option:hover:not(:disabled) {
        transform: translateY(-2px);
        box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
      }

      /* Animation cho màn hình kết quả */
      @keyframes popIn {
        0% {
          transform: scale(0.8);
          opacity: 0;
        }
        100% {
          transform: scale(1);
          opacity: 1;
        }
      }
      .animate-pop-in {
        animation: popIn 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
      }

      /* Slider styling */
      input[type="range"]::-webkit-slider-thumb {
        -webkit-appearance: none;
        height: 20px;
        width: 20px;
        border-radius: 50%;
        background: #d97706;
        cursor: pointer;
        margin-top: -8px;
        box-shadow: 0 0 2px rgba(0, 0, 0, 0.2);
      }
      input[type="range"]::-webkit-slider-runnable-track {
        width: 100%;
        height: 4px;
        cursor: pointer;
        background: #fed7aa;
        border-radius: 2px;
      }

      /* AI Markdown Styling */
      .prose p {
        margin-bottom: 0.5em;
      }
      .prose ul {
        list-style-type: disc;
        margin-left: 1.5em;
        margin-bottom: 0.5em;
      }
      .prose strong {
        color: #b45309;
      }
    </style>
  </head>
  <body class="flex flex-col min-h-screen">
    <!-- Navigation -->
    <nav class="bg-white shadow-md sticky top-0 z-50">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between h-16">
          <div class="flex items-center">
            <span class="text-2xl font-bold text-amber-600 mr-2">⚖️</span>
            <span class="text-xl font-bold text-gray-800 hidden md:block"
              >Pháp Luật By Hiếu</span
            >
          </div>
          <div class="flex items-center space-x-2 md:space-x-4 overflow-x-auto">
            <button
              onclick="switchView('dashboard')"
              class="px-3 py-2 rounded-md text-sm font-medium hover:bg-amber-50 text-gray-700 transition whitespace-nowrap"
              id="nav-dashboard"
            >
              Tổng Quan
            </button>
            <button
              onclick="switchView('practice')"
              class="px-3 py-2 rounded-md text-sm font-medium hover:bg-amber-50 text-gray-700 transition whitespace-nowrap"
              id="nav-practice"
            >
              Tra Cứu
            </button>
            <button
              onclick="switchView('ai-chat')"
              class="px-3 py-2 rounded-md text-sm font-medium hover:bg-purple-50 text-purple-700 transition flex items-center whitespace-nowrap"
              id="nav-ai-chat"
            >
              <span class="mr-1">✨</span> Trợ Lý AI
            </button>
            <button
              onclick="switchView('quiz-setup')"
              class="px-3 py-2 rounded-md text-sm font-medium bg-amber-600 text-white hover:bg-amber-700 shadow transition whitespace-nowrap"
              id="nav-quiz"
            >
              Thi Thử
            </button>
          </div>
        </div>
      </div>
    </nav>

    <!-- Main Content -->
    <main class="flex-grow container mx-auto px-4 py-8">
      <!-- Loading State -->
      <div id="loading" class="text-center py-20">
        <div
          class="animate-spin rounded-full h-12 w-12 border-b-2 border-amber-600 mx-auto mb-4"
        ></div>
        <p class="text-lg text-gray-600">Đang tải dữ liệu câu hỏi...</p>
      </div>

      <!-- VIEW: DASHBOARD -->
      <div id="view-dashboard" class="hidden space-y-8 fade-in">
        <!-- Intro Section -->
        <div
          class="bg-white rounded-2xl shadow-sm p-8 border-l-4 border-amber-500"
        >
          <h1 class="text-3xl font-bold text-gray-900 mb-4">
            Hệ thống ôn tập thông minh
          </h1>
          <p class="text-gray-600 leading-relaxed mb-6">
            Ứng dụng này tổng hợp <b>162 câu hỏi trắc nghiệm</b> môn Pháp luật
            đại cương. Mới: Tích hợp <b>Gemini AI API</b> Luyện Đề Cương ,giải
            thích đáp án và giải đáp thắc mắc pháp luật của bạn ngay lập tức.
          </p>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
            <div class="bg-amber-50 p-4 rounded-xl border border-amber-100">
              <p
                class="text-sm text-amber-800 font-semibold uppercase tracking-wider"
              >
                Tổng số câu hỏi
              </p>
              <p class="text-3xl font-bold text-amber-600" id="stat-total-q">
                0
              </p>
            </div>
            <div class="bg-blue-50 p-4 rounded-xl border border-blue-100">
              <p
                class="text-sm text-blue-800 font-semibold uppercase tracking-wider"
              >
                Số chương
              </p>
              <p class="text-3xl font-bold text-blue-600" id="stat-total-c">
                0
              </p>
            </div>
            <div class="bg-purple-50 p-4 rounded-xl border border-purple-100">
              <p
                class="text-sm text-purple-800 font-semibold uppercase tracking-wider"
              >
                Trí tuệ nhân tạo
              </p>
              <p class="text-3xl font-bold text-purple-600">Gemini API</p>
            </div>
          </div>
        </div>

        <!-- Chart Section -->
        <div class="bg-white rounded-2xl shadow-sm p-6">
          <h2 class="text-xl font-bold text-gray-800 mb-6 border-b pb-2">
            Phân Bố Câu Hỏi Theo Chương
          </h2>
          <div class="chart-container">
            <canvas id="chapterChart"></canvas>
          </div>
        </div>
      </div>

      <!-- VIEW: PRACTICE (EXPLORE) -->
      <div id="view-practice" class="hidden space-y-6 fade-in">
        <div
          class="flex flex-col md:flex-row justify-between items-center bg-white p-4 rounded-xl shadow-sm sticky top-20 z-40 border border-gray-100"
        >
          <div class="mb-4 md:mb-0">
            <h2 class="text-xl font-bold text-gray-800">
              Tra Cứu & Ôn Tập Câu hỏi
            </h2>
            <p class="text-sm text-gray-500">
              Tìm kiếm và xem giải thích chi tiết từ AI.
            </p>
          </div>
          <div class="w-full md:w-1/2 relative">
            <input
              type="text"
              id="searchInput"
              placeholder="Tìm kiếm nội dung câu hỏi..."
              class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent outline-none transition"
            />
            <span class="absolute left-3 top-2.5 text-gray-400">🔍</span>
          </div>
        </div>

        <div id="questions-list" class="space-y-4">
          <!-- Questions injected here by JS -->
        </div>

        <div id="no-results" class="hidden text-center py-10 text-gray-500">
          Không tìm thấy câu hỏi nào phù hợp với từ khóa.
        </div>
      </div>

      <!-- VIEW: AI CHAT ASSISTANT -->
      <div
        id="view-ai-chat"
        class="hidden h-[calc(100vh-140px)] flex flex-col fade-in max-w-4xl mx-auto"
      >
        <div
          class="bg-gradient-to-r from-purple-600 to-indigo-600 p-6 rounded-t-2xl shadow-lg text-white flex-shrink-0"
        >
          <h2 class="text-2xl font-bold flex items-center">
            <span class="text-3xl mr-3">✨</span> Trợ Lý Luật Sư AI
          </h2>
          <p class="text-purple-100 opacity-90 mt-1">
            Hỏi bất cứ điều gì về Pháp luật đại cương. Tôi sẽ giải thích dựa
            trên Hiến pháp và pháp luật Việt Nam.
          </p>
        </div>

        <!-- Chat History -->
        <div
          id="chat-history"
          class="flex-grow bg-white border-x border-gray-200 overflow-y-auto p-4 space-y-4"
        >
          <div class="flex justify-start">
            <div
              class="bg-gray-100 rounded-2xl rounded-tl-none px-4 py-3 max-w-[80%] text-gray-800"
            >
              Xin chào! Bạn cần giải thích thuật ngữ nào hoặc muốn tìm hiểu về
              chương nào trong môn Pháp luật đại cương?
            </div>
          </div>
        </div>

        <!-- Input Area -->
        <div
          class="bg-white p-4 border border-t-0 border-gray-200 rounded-b-2xl shadow-lg flex-shrink-0"
        >
          <div class="flex gap-2">
            <input
              type="text"
              id="chat-input"
              class="flex-grow p-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-purple-500 focus:border-transparent outline-none"
              placeholder="Nhập câu hỏi của bạn tại đây..."
              onkeypress="if (event.key === 'Enter') sendChatMessage();"
            />
            <button
              onclick="sendChatMessage()"
              id="btn-send-chat"
              class="bg-purple-600 text-white px-6 py-3 rounded-xl font-bold hover:bg-purple-700 transition flex items-center"
            >
              <span>Gửi</span>
            </button>
          </div>
        </div>
      </div>

      <!-- VIEW: QUIZ SETUP -->
      <div
        id="view-quiz-setup"
        class="hidden h-full flex items-center justify-center fade-in py-10"
      >
        <div
          class="bg-white p-8 rounded-2xl shadow-lg max-w-2xl w-full border-t-8 border-amber-500"
        >
          <div class="text-center mb-8">
            <h2 class="text-3xl font-bold text-gray-900 mb-2">
              Thiết Lập Bài Thi
            </h2>
            <p class="text-gray-600">
              Tùy chỉnh bài thi để kiểm tra kiến thức của bạn.
            </p>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
            <!-- Column 1: Config -->
            <div class="space-y-6">
              <!-- Question Count Slider -->
              <div class="bg-amber-50 p-5 rounded-xl border border-amber-100">
                <label
                  class="block text-sm font-bold text-gray-700 mb-3 flex justify-between"
                >
                  <span>Số lượng câu hỏi</span>
                  <span class="text-amber-600 text-lg" id="setup-count-display"
                    >20 câu</span
                  >
                </label>
                <input
                  type="range"
                  id="setup-count"
                  min="10"
                  max="162"
                  value="20"
                  class="w-full h-2 bg-amber-200 rounded-lg appearance-none cursor-pointer"
                  oninput="
                    document.getElementById('setup-count-display').innerText =
                      this.value + ' câu'
                  "
                />
              </div>

              <!-- Time Input -->
              <div class="bg-blue-50 p-5 rounded-xl border border-blue-100">
                <label class="block text-sm font-bold text-gray-700 mb-3"
                  >Thời gian làm bài (Phút)</label
                >
                <div class="flex items-center space-x-2">
                  <input
                    type="number"
                    id="setup-time"
                    min="0"
                    value="15"
                    class="w-full p-2 border border-blue-200 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-center font-bold text-lg text-blue-800"
                  />
                  <span class="text-sm text-gray-500 whitespace-nowrap"
                    >(0 = Không giới hạn)</span
                  >
                </div>
              </div>
            </div>

            <!-- Column 2: Quick Start & Action -->
            <div class="flex flex-col justify-between space-y-4">
              <div class="bg-gray-50 p-5 rounded-xl border border-gray-200">
                <h3 class="font-bold text-gray-700 mb-3">Chế độ nhanh</h3>
                <div class="grid grid-cols-2 gap-3">
                  <button
                    onclick="setQuickConfig(20, 15)"
                    class="py-2 bg-white border border-gray-300 rounded hover:bg-gray-100 text-sm"
                  >
                    20 câu / 15p
                  </button>
                  <button
                    onclick="setQuickConfig(40, 30)"
                    class="py-2 bg-white border border-gray-300 rounded hover:bg-gray-100 text-sm"
                  >
                    40 câu / 30p
                  </button>
                  <button
                    onclick="setQuickConfig(50, 45)"
                    class="py-2 bg-white border border-gray-300 rounded hover:bg-gray-100 text-sm"
                  >
                    50 câu / 45p
                  </button>
                  <button
                    onclick="setQuickConfig(162, 0)"
                    class="py-2 bg-white border border-gray-300 rounded hover:bg-gray-100 text-sm"
                  >
                    Tất cả / No limit
                  </button>
                </div>
              </div>

              <button
                onclick="startCustomQuiz()"
                class="w-full py-4 bg-gradient-to-r from-amber-500 to-orange-600 hover:from-amber-600 hover:to-orange-700 text-white rounded-xl font-bold shadow-md transform transition hover:-translate-y-1 flex items-center justify-center space-x-2"
              >
                <span>🚀 Bắt Đầu Làm Bài</span>
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- VIEW: QUIZ ACTIVE -->
      <div id="view-quiz-active" class="hidden max-w-3xl mx-auto fade-in pb-20">
        <!-- Progress Header with Timer -->
        <div
          class="bg-white rounded-xl shadow-sm p-4 mb-6 sticky top-20 z-30 border border-gray-100"
        >
          <div class="flex justify-between items-center mb-2">
            <div class="flex items-center space-x-4">
              <div>
                <span
                  class="text-xs font-bold text-gray-400 uppercase tracking-wide"
                  >Tiến độ</span
                >
                <div class="text-xl font-bold text-amber-600">
                  <span id="quiz-current">1</span> /
                  <span id="quiz-total">20</span>
                </div>
              </div>

              <!-- Timer Display -->
              <div
                id="timer-container"
                class="hidden pl-4 border-l border-gray-200"
              >
                <span
                  class="text-xs font-bold text-gray-400 uppercase tracking-wide"
                  >Thời gian</span
                >
                <div
                  class="text-xl font-mono font-bold text-red-600"
                  id="quiz-timer"
                >
                  00:00
                </div>
              </div>
            </div>

            <div class="text-right">
              <span
                class="text-xs font-bold text-gray-400 uppercase tracking-wide"
                >Điểm số</span
              >
              <div class="text-xl font-bold text-green-600" id="quiz-score">
                0
              </div>
            </div>
          </div>
          <!-- Progress Bar -->
          <div class="w-full bg-gray-200 rounded-full h-2.5">
            <div
              id="quiz-progress-bar"
              class="bg-amber-600 h-2.5 rounded-full transition-all duration-300"
              style="width: 0%"
            ></div>
          </div>
        </div>

        <!-- Question Card -->
        <div
          class="bg-white rounded-2xl shadow-lg overflow-hidden min-h-[400px] flex flex-col relative"
        >
          <div class="bg-gray-50 p-6 border-b border-gray-100">
            <span
              class="inline-block px-3 py-1 bg-amber-100 text-amber-800 text-xs font-bold rounded-full mb-2"
              id="quiz-chapter-tag"
              >Chương 1</span
            >
            <h3
              class="text-lg md:text-xl font-semibold text-gray-900 leading-snug"
              id="quiz-question-text"
            >
              ...
            </h3>
          </div>

          <div
            class="p-6 flex-grow flex flex-col justify-center space-y-3"
            id="quiz-options-container"
          >
            <!-- Options injected here -->
          </div>

          <!-- Feedback Area -->
          <div
            id="quiz-feedback"
            class="hidden p-4 bg-gray-50 border-t border-gray-100 text-center"
          >
            <p id="quiz-feedback-text" class="font-bold mb-2"></p>
            <button
              onclick="nextQuestion()"
              class="px-6 py-2 bg-blue-600 text-white font-bold rounded-lg hover:bg-blue-700 transition shadow-lg"
            >
              Câu Tiếp Theo ➜
            </button>
          </div>
        </div>
        <div class="text-center mt-6">
          <button
            onclick="endQuiz()"
            class="text-gray-400 hover:text-red-500 text-sm font-medium transition flex items-center justify-center mx-auto"
          >
            Kết thúc bài thi sớm
          </button>
        </div>
      </div>

      <!-- VIEW: QUIZ RESULT (ACHIEVEMENT) -->
      <div
        id="view-quiz-result"
        class="hidden h-full flex flex-col items-center justify-center fade-in py-10 px-4"
      >
        <!-- Result Card -->
        <div
          class="bg-white w-full max-w-lg rounded-3xl shadow-2xl overflow-hidden border-4 border-amber-400 animate-pop-in relative"
        >
          <!-- Confetti decoration -->
          <div
            class="absolute top-0 left-0 w-full h-full overflow-hidden pointer-events-none"
          >
            <div class="absolute top-10 left-10 text-2xl animate-bounce">
              ✨
            </div>
            <div
              class="absolute top-20 right-10 text-xl animate-bounce"
              style="animation-delay: 0.2s"
            >
              🎉
            </div>
            <div
              class="absolute bottom-10 left-20 text-2xl animate-bounce"
              style="animation-delay: 0.5s"
            >
              🎊
            </div>
          </div>

          <!-- Header Gradient -->
          <div
            class="bg-gradient-to-r from-amber-500 to-orange-600 p-8 text-center text-white relative z-10"
          >
            <div
              class="text-sm font-semibold uppercase tracking-widest opacity-80 mb-2"
            >
              KẾT QUẢ BÀI THI
            </div>
            <div class="text-6xl mb-2" id="result-emoji">🏆</div>
            <h2 class="text-3xl font-extrabold" id="result-title">XUẤT SẮC!</h2>
            <p class="text-amber-100 mt-2" id="result-subtitle">
              Bạn đã nắm rất chắc kiến thức.
            </p>
          </div>

          <!-- Stats Body -->
          <div class="p-8">
            <div class="flex justify-center items-center mb-8">
              <div class="text-center w-1/2 border-r border-gray-200">
                <div class="text-gray-400 text-xs uppercase font-bold mb-1">
                  Điểm Số
                </div>
                <div class="text-4xl font-black text-gray-800">
                  <span id="result-score">0</span>/<span id="result-total"
                    >0</span
                  >
                </div>
              </div>
              <div class="text-center w-1/2">
                <div class="text-gray-400 text-xs uppercase font-bold mb-1">
                  Chính Xác
                </div>
                <div
                  class="text-4xl font-black text-green-500"
                  id="result-percent"
                >
                  0%
                </div>
              </div>
            </div>

            <div class="bg-gray-50 rounded-xl p-4 mb-6 border border-gray-100">
              <div class="flex justify-between items-center mb-2">
                <span class="text-sm text-gray-500"
                  >⏱️ Thời gian hoàn thành:</span
                >
                <span class="font-bold text-gray-800" id="result-time"
                  >00:00</span
                >
              </div>
              <div class="flex justify-between items-center">
                <span class="text-sm text-gray-500">📚 Số câu đã làm:</span>
                <span class="font-bold text-gray-800" id="result-answered"
                  >0</span
                >
              </div>
            </div>

            <div class="space-y-3">
              <button
                onclick="switchView('dashboard')"
                class="w-full py-3 bg-gray-900 text-white rounded-xl font-bold hover:bg-gray-800 transition shadow-lg"
              >
                🏠 Về Trang Chủ
              </button>
              <button
                onclick="switchView('quiz-setup')"
                class="w-full py-3 bg-white text-gray-700 border-2 border-gray-200 rounded-xl font-bold hover:bg-gray-50 transition"
              >
                🔄 Thi Lại
              </button>
            </div>
          </div>
        </div>

        <p class="text-gray-400 text-sm mt-6 text-center italic">
          "Chụp màn hình lại để khoe thành tích nhé!"
        </p>
      </div>
    </main>

    <footer class="bg-white border-t border-gray-200 mt-auto">
      <div class="max-w-7xl mx-auto py-6 px-4 text-center">
        <p class="text-gray-500 text-sm">
          © 2024 Ứng dụng ôn tập Pháp Luật Đại Cương.
        </p>
      </div>
    </footer>

    <!-- RAW DATA STORAGE -->
    <script>
      const apiKey = "AIzaSyDcelyvUElkstMfV1Icd4rT5Ygn0p6tV30";

      const rawMarkdown = `
## CHƯƠNG 1

**Câu 1:** Theo học thuyết Mác – Lênin, nhận định nào sau đây là đúng:
A. Tính chất giai cấp của nhà nước không đổi nhưng bản chất của nhà nước thì thay đổi qua các kiểu nhà nước khác nhau.
B. Tính chất giai cấp và bản chất của nhà nước không thay đổi qua các kiểu nhà nước khác nhau.
C. Tính chất giai cấp và bản chất của nhà nước luôn luôn thay đổi qua các kiểu nhà nước khác nhau.
*D. Tính chất giai cấp của nhà nước luôn luôn thay đổi, còn bản chất của nhà nước là không đổi qua các kiểu nhà nước khác nhau.

**Câu 2:** Thuộc tính nào sau đây là thuộc tính của pháp luật:
A. Tính được đảm bảo thực hiện bằng nhà nước.
B. Tính bắt buộc chung (hay tính quy phạm phổ biến).
C. Tính xác định chặt chẽ về mặt hình thức.
*D. Tất cả các đáp án đều đúng.

**Câu 3:** Chức năng nào sau đây là chức năng của pháp luật:
A. Chức năng lập hiến và lập pháp.
B. Chức năng giám sát tối cao.
*C. Chức năng điều chỉnh các Quan hệ xã hội.
D. Tất cả các chức năng trên đều đúng.

**Câu 4:** Lịch sử xã hội loài người đã và đang trải qua 5 hình thái kinh tế - xã hội, tương ứng với mấy kiểu nhà nước:
A. 3 kiểu nhà nước.
*B. 4 kiểu nhà nước.
C. 5 kiểu nhà nước.
D. 6 kiểu nhà nước.

**Câu 5:** Mục đích tồn tại của nhà nước là:
A. Bảo vệ lợi ích của giai cấp thống trị.
B. Duy trì trật tự và quản lý xã hội.
C. Sự thống trị của giai cấp này đối với giai cấp khác.
*D. Tất cả các đáp án đều đúng.

**Câu 6:** Nguyên nhân dẫn đến sự ra đời và tồn tại của Nhà nước:
A. Là kết quả tất yếu của xã hội loài người, khi xã hội xuất hiện tư hữu về tư liệu sản xuất.
B. Là kết quả tất yếu của xã hội có giai cấp.
C. Là do ý chí của giai cấp cầm quyền với mong muốn thành lập nên nhà nước để bảo vệ lợi ích của họ.
*D. Tất cả các đáp án trên đều đúng.

**Câu 7:** Ngoài tính chất giai cấp, kiểu nhà nước nào sau đây còn có vai trò xã hội:
A. Nhà nước XHCN.
B. Nhà nước tư sản.
C. Nhà nước phong kiến.
*D. Tất cả các đáp trên đều đúng.

**Câu 8:** Lịch sử xã hội loài người đã và đang trải qua mấy kiểu pháp luật:
A. 2 Kiểu pháp luật.
B. 3 Kiểu pháp luật.
*C. 4 Kiểu pháp luật.
D. 5 Kiểu pháp luật.

**Câu 9:** Chức năng nào không phải là chức năng của pháp luật:
A. Chức năng điều chỉnh các Quan hệ xã hội.
*B. Chức năng xây dựng và bảo vệ tổ quốc.
C. Chức năng bảo vệ các Quan hệ xã hội.
D. Chức năng giáo dục.

**Câu 10:** Thuộc tính nào sau đây không phải là thuộc tính của pháp luật?
A. Tính bắt buộc chung (hay tính quy phạm phổ biến).
B. Tính xác định chặt chẽ về mặt hình thức.
*C. Tính giáo dục.
D. Tính cưỡng chế.

**Câu 11:** Con đường hình thành nên pháp luật:
A. Sáng tạo pháp luật.
B. Sáng tạo pháp luật và tập quán pháp.
C. Sáng tạo pháp luật và tiền lệ pháp.
*D. Sáng tạo pháp luật hoặc tập quán pháp và tiền lệ pháp.

**Câu 12:** Việc tòa án thường đưa các vụ án xét xử lưu động thể hiện chủ yếu chức năng nào của pháp luật:
A. Chức năng điều chỉnh các Quan hệ xã hội.
B. Chức năng bảo vệ các Quan hệ xã hội.
*C. Chức năng giáo dục pháp luật.
D. Cả ba đáp án trên đều sai.

**Câu 13:** Theo chủ nghĩa Mác – Lênin, khái niệm “chế độ cộng sản nguyên thủy” dùng để chỉ:
A. Một kiểu nhà nước.
*B. Một hình thái kinh tế xã hội.
C. Cả một kiểu nhà nước và hình thái kinh tế xã hội.
D. Tất cả các đáp án đều sai.

**Câu 14:** Chức năng nào không phải là chức năng của pháp luật:
A. Chức năng điều chỉnh các Quan hệ xã hội.
*B. Chức năng lập hiến và lập pháp.
C. Chức năng bảo vệ các Quan hệ xã hội.
D. Chức năng giáo dục.

## CHƯƠNG 2

**Câu 15:** Văn bản nào có hiệu lực pháp lý cao nhất trong Hệ thống văn bản Quy phạm pháp luật sau đây:
A. Pháp lệnh.
B. Luật.
*C. Hiến pháp.
D. Nghị quyết.

**Câu 16:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Nghị định:
A. Quốc hội.
*B. Chính phủ.
C. Bộ trưởng các Bộ.
D. Tòa án nhân dân.

**Câu 17:** Ủy ban Thường vụ Quốc hội có quyền ban hành những loại Văn bản Quy phạm pháp luật nào:
A. Luật, nghị quyết.
B. Luật, pháp lệnh.
*C. Pháp lệnh, nghị quyết.
D. Pháp lệnh, nghị quyết, nghị định.

**Câu 18:** Có thể thay đổi hiệu lực của Văn bản Quy phạm pháp luật bằng cách:
A. Ban hành mới Văn bản pháp luật.
B. Sửa đổi, bổ sung các Văn bản pháp luật hiện hành.
C. Đình chỉ, bãi bỏ các Văn bản pháp luật hiện hành.
*D. Tất cả các đáp án đều đúng.

**Câu 19:** Cơ quan nào là cơ quan hành chính nhà nước cao nhất của nước CHXNCN Việt Nam
A. Chủ tịch nước.
*B. Chính phủ.
C. Quốc hội.
D. Tòa án nhân dân và Viện kiểm sát nhân dân.

**Câu 20:** Tính xác định chặt chẽ về mặt hình thức là thuộc tính của:
A. Quy phạm đạo đức.
B. Quy phạm tập quán.
C. Quy phạm tôn giáo.
*D. Quy phạm pháp luật.

**Câu 21:** Thủ tướng chính phủ có quyền ban hành những loại Văn bản Quy phạm pháp luật nào:
A. Nghị định, quyết định.
B. Nghị định, quyết định, chỉ thị.
C. Quyết định, chỉ thị, thông tư.
*D. Quyết định.

**Câu 22:** Bộ trưởng có quyền ban hành những loại Văn bản Quy phạm pháp luật nào:
A. Nghị quyết, quyết định.
B. Pháp lệnh, quyết định.
C. Nghị định, quyết định.
*D. Thông tư.

**Câu 23:** Văn bản nào có hiệu lực cao nhất trong các văn bản sau:
A. Thông tư.
*B. Bộ Luật.
C. Pháp lệnh.
D. Chỉ thị.

**Câu 24:** Về mặt cấu trúc, mỗi một Quy phạm pháp luật:
A. Phải có cả ba bộ phận cấu thành: giả định, quy định, chế tài.
*B. Phải có ít nhất hai bộ phận trong ba bộ phận sau: giả định, quy định, chế tài.
C. Chỉ cần có một trong ba bộ phận: giả định, quy định, chế tài.
D. Tất cả các đáp án đều sai.

**Câu 25:** Văn bản nào có hiệu lực cao nhất trong các văn bản sau:
*A. Luật.
B. Pháp lệnh.
C. Thông tư.
D. Chỉ thị.

**Câu 26:** Phần quy định của Quy phạm pháp luật được hiểu:
*A. Là quy tắc xử sự mà mọi chủ thể phải tuân theo khi xuất hiện những điều kiện mà Quy phạm pháp luật đã dự kiến trước.
B. Nêu lên đặc điểm, thời gian, chủ thể, tình huống, điều kiện, hoàn cảnh có thể xảy ra trong thực tế.
C. Chỉ ra những biện pháp tác động mà nhà nước sẽ áp dụng đối với các chủ thể không thực hiện hoặc thực hiện không đúng mệnh lệnh đã nêu.
D. Tất cả các đáp án đều đúng.

**Câu 27:** Quy phạm nào có chức năng điều chỉnh các Quan hệ xã hội:
A. Quy phạm đạo đức.
B. Quy phạm tập quán.
C. Quy phạm tôn giáo.
*D. Tất cả các đáp án đều đúng.

**Câu 28:** Đạo luật nào điều chỉnh việc ban hành Văn bản Quy phạm pháp luật:
A. Luật tổ chức chính phủ.
B. Hiến pháp.
C. Luật tổ chức quốc hội.
*D. Luật ban hành Văn bản Quy phạm pháp luật.

**Câu 29:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Thông tư:
A. Quốc hội.
B. Chính phủ.
*C. Bộ trưởng các Bộ.
D. Tổng kiểm toán Nhà nước.

**Câu 30:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Nghị quyết:
*A. Quốc hội.
B. Chính phủ.
C. Bộ trưởng các Bộ.
D. Tòa án nhân dân.

**Câu 31:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Nghị quyết:
A. Chính phủ.
B. Chánh án Tòa án nhân dân tối cao.
C. Viện trưởng viện Kiểm sát nhân dân tối cao.
*D. Hội đồng thẩm phán của Tòa án nhân dân tối cao.

**Câu 32:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Thông tư:
A. Quốc hội.
B. Chính phủ.
*C. Chánh án tòa án nhân dân tối cao.
D. Đài truyền hình Việt Nam.

**Câu 33:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Thông tư:
A. Quốc hội.
B. Chính phủ.
*C. Viện trưởng Viện kiểm sát nhân dân tối cao.
D. Văn phòng chủ tịch nước.

**Câu 34:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Pháp lệnh:
A. Chính phủ.
B. Chánh án Tòa án nhân dân tối cao.
C. Viện trưởng viện Kiểm sát nhân dân tối cao.
*D. Ủy ban thường vụ Quốc hội.

**Câu 35:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Luật:
A. Chính phủ.
B. Chánh án Tòa án nhân dân tối cao.
*C. Quốc hội.
D. Ủy ban thường vụ Quốc hội.

**Câu 36:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Quyết định:
A. Chính phủ.
B. Ủy ban thường vụ Quốc hội.
C. Quốc hội.
*D. Chủ tịch nước.

**Câu 37:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Quyết định:
*A. Tất cả các đáp án đều đúng.
B. Bộ trưởng.
C. Chánh án Tòa án nhân dân tối cao.
D. Thủ tướng Chính phủ.

**Câu 38:** Theo quy định của Luật ban hành văn bản quy phạm pháp luật 2015, cơ quan nào sau đây có quyền ban hành Lệnh:
A. Thủ tướng Chính phủ.
B. Bộ trưởng.
C. Chánh án Tòa án nhân dân tối cao.
*D. Chủ tịch nước.

**Câu 39:** Trong Hệ thống pháp luật Việt Nam, để được coi là một ngành luật độc lập thì:
A. Ngành luật đó phải có đối tượng điều chỉnh riêng.
B. Ngành luật đó phải có phương pháp điều chỉnh riêng.
*C. Ngành luật đó phải có đối tượng điều chỉnh và phương pháp điều chỉnh riêng.
D. Tất cả các đáp án đều sai.

**Câu 40:** UBND và chủ tịch UBND các cấp có quyền ban hành những loại VBPL nào:
A. Nghị định, quyết định.
*B. Quyết định, chỉ thị.
C. Quyết định, chỉ thị, thông tư.
D. Nghị định, nghị quyết, quyết định, chỉ thị.

**Câu 41:** Hội đồng nhân dân các cấp có quyền ban hành loại Văn bản nào sau đây:
*A. Nghị quyết.
B. Nghị định.
C. Nghị quyết, nghị định.
D. Nghị quyết, nghị định, quyết định.

**Câu 42:** Chế tài của Quy phạm pháp luật là:
A. Hình phạt nghiêm khắc của nhà nước đối với người có hành vi vi phạm pháp luật.
B. Những hậu quả pháp lý bất lợi có thể áp dụng đối với người không thực hiện hoặc thực hiện không đúng quy định của Quy phạm pháp luật.
C. Những biện pháp, tác động mà nhà nước dự kiến áp dụng đối với chủ thể vi phạm pháp luật.
*D. Tất cả các đáp án đều đúng.

**Câu 43:** Phần giả định của Quy phạm pháp luật là:
A. Quy tắc xử sự thể hiện ý chí của nhà nước mà mọi người phải thi hành khi xuất hiện những điều kiện mà QPPL đã dự kiến trước.
B. Chỉ ra những biện pháp tác động mà nhà nước sẽ áp dụng đối với các chủ thể không thực hiện hoặc thực hiện không đúng mệnh lệnh của nhà nước đã nêu trong phần quy định.
*C. Nêu lên đặc điểm, thời gian, chủ thể, tình huống, điều kiện, hoàn cảnh có thể xảy ra trong thực tế, là môi trường tác động của Quy phạm pháp luật.
D. Tất cả các đáp án đều đúng.

**Câu 44:** Văn bản nào có hiệu lực cao nhất trong trong số các loại văn bản sau:
*A. Pháp lệnh.
B. Thông tư.
C. Chỉ thị.
D. Nghị định.

**Câu 45:** Văn bản nào có hiệu lực cao nhất trong trong số các loại văn bản sau:
A. Quyết định của Thủ tướng Chính phủ.
B. Thông tư.
C. Chỉ thị.
*D. Nghị quyết của Quốc hội.

**Câu 46:** Văn bản nào có hiệu lực cao nhất trong trong số các loại văn bản sau:
A. Lệnh.
B. Thông tư.
*C. Luật.
D. Nghị quyết của Quốc hội.

**Câu 47:** Văn bản nào có hiệu lực cao nhất trong trong số các loại văn bản sau:
A. Quyết định của Chủ tịch nước.
B. Thông tư.
C. Nghị định.
*D. Nghị quyết của Quốc hội.

**Câu 48:** Đạo luật nào dưới đây quy định một cách cơ bản về chế độ chính trị, chế độ kinh tế, văn hóa, xã hội và tổ chức bộ máy Nhà nước:
A. Luật tổ chức Quốc hội.
B. Luật tổ chức Chính phủ.
C. Luật tổ chức Hội đồng nhân dân và UBND.
*D. Hiến pháp.

**Câu 49:** Trong một Quy phạm pháp luật:
A. Phải có đầy đủ cả ba yếu tố cấu thành.
*B. Có thể chỉ có hai yếu tố cấu thành.
C. Có thể chỉ có một yếu tố cấu thành.
D. Tất cả các đáp án đều đúng.

**Câu 50:** Nguyên tắc “không áp dụng hiệu lực hồi tố” của Văn bản quy phạm pháp luật được hiểu là:
A. Văn bản quy phạm pháp luật chỉ áp dụng trong phạm vi lãnh thổ Việt Nam.
B. Văn bản quy phạm pháp luật chỉ áp dụng trong một khoảng thời gian nhất định.
*C. Văn bản quy phạm pháp luật không áp dụng đối với những hành vi xảy ra trước thời điểm văn bản đó có hiệu lực pháp luật.
D. Văn bản quy phạm pháp luật áp dụng trong phạm vi lãnh thổ Việt Nam và trong một khoảng thời gian nhất định.

**Câu 51:** Quy phạm pháp luật là cách xử sự do nhà nước quy định để:
A. Áp dụng cho một lần duy nhất và hết hiệu lực sau lần áp dụng đó.
B. Áp dụng cho một lần duy nhất và vẫn còn hiệu lực sau lần áp dụng đó.
*C. Áp dụng cho nhiều lần và vẫn còn hiệu lực sau những lần áp dụng đó.
D. Áp dụng cho nhiều lần và hết hiệu lực sau những lần áp dụng đó.

**Câu 52:** Dấu hiệu của Văn bản Quy phạm pháp luật là:
A. Có tính bắt buộc chung.
B. Được áp dụng nhiều lần.
C. Do các cơ quan Nhà nước có thẩm quyền ban hành.
*D. Tất cả các đáp án đều đúng.

## CHƯƠNG 3

**Câu 53:** Điều kiện để một tổ chức tham gia vào một Quan hệ pháp luật cụ thể:
A. Chỉ cần có Năng lực hành vi.
B. Chỉ cần có Năng lực pháp luật.
*C. Có năng lực chủ thể.
D. Tất cả các đáp án đều sai.

**Câu 54:** Điều kiện để làm phát sinh, thay đổi hay chấm dứt một Quan hệ pháp luật?
A. Khi có Quy phạm pháp luật điều chỉnh Quan hệ pháp luật tương ứng.
B. Khi xảy ra sự kiện pháp lý.
C. Khi xuất hiện chủ thể pháp luật trong trường hợp cụ thể.
*D. Tất cả các đáp án đều đúng.

**Câu 55:** Khẳng định nào sau đây là đúng:
*A. Sự kiện pháp lý là sự cụ thể hoá phần giả định của Quy phạm pháp luật trong thực tiễn.
B. Sự kiện pháp lý là sự cụ thể hoá phần giả định và quy định của Quy phạm pháp luật trong thực tiễn.
C. Sự kiện pháp lý là sự cụ thể hoá phần giả định, quy định và chế tài của Quy phạm pháp luật trong thực tiễn.
D. Tất cả các đáp án đều đúng.

**Câu 56:** Điều kiện để trở thành chủ thể của Quan hệ pháp luật:
*A. Có năng lực chủ thể.
B. Có Năng lực hành vi.
C. Có Năng lực pháp luật.
D. Tất cả các đáp án đều sai.

**Câu 57:** Năng lực pháp luật của chủ thể Quan hệ pháp luật là:
*A. Khả năng của chủ thể có được các quyền chủ thể và mang các nghĩa vụ pháp lý mà nhà nước thừa nhận.
B. Khả năng của chủ thể được nhà nước thừa nhận, bằng các hành vi của mình thực hiện các quyền chủ thể và nghĩa vụ pháp lý, tham gia vào các Quan hệ pháp luật.
C. Khả năng nhận thức, khả năng điều khiển hành vi và đạt độ tuổi nhất định.
D. Tất cả các đáp án đều sai.

**Câu 58:** Điều kiện để một tổ chức được coi là pháp nhân:
A. Được thành lập hợp pháp; Có cơ cấu tổ chức chặt chẽ.
B. Có tài sản độc lập với tài sản của tổ chức, cá nhân khác và tự chịu trách nhiệm bằng tài sản đó.
C. Nhân danh mình tham gia vào các Quan hệ pháp luật một cách độc lập.
*D. Được thành lập hợp pháp; có cơ cấu tổ chức chặt chẽ, có tài sản độc lập với tài sản của tổ chức, cá nhân khác và tự chịu trách nhiệm bằng tài sản đó; nhân danh mình tham gia vào các Quan hệ pháp luật một cách độc lập.

**Câu 59:** Chủ thể của Quan hệ pháp luật là:
A. Bất kỳ cá nhân, tổ chức nào trong một nhà nước.
*B. Cá nhân, tổ chức được nhà nước công nhận có khả năng tham gia vào các Quan hệ pháp luật.
C. Cá nhân, tổ chức cụ thể có được những quyền và mang những nghĩa vụ pháp lý nhất định được chỉ ra trong các Quan hệ pháp luật cụ thể.
D. Tất cả các đáp án đều sai.

**Câu 60:** Khẳng định nào sau đây là đúng:
A. Các quy phạm xã hội không phải là Quy phạm pháp luật cũng mang tính bắt buộc chung.
*B. Quy phạm pháp luật mang tính bắt buộc chung.
C. Các quy phạm xã hội không phải là Quy phạm pháp luật cũng mang tính cưỡng chế nhưng không mang tính bắt buộc chung.
D. Tất cả các đáp án đều sai.

**Câu 61:** Năng lực hành vi của chủ thể Quan hệ pháp luật là:
A. Khả năng của chủ thể có được các quyền chủ thể và mang các nghĩa vụ pháp lý mà nhà nước thừa nhận.
*B. Khả năng của chủ thể được nhà nước thừa nhận, bằng các hành vi của mình thực hiện các quyền chủ thể và nghĩa vụ pháp lý, tham gia vào các Quan hệ pháp luật.
C. Khả năng của chủ thể được nhà nước cho phép tham gia các Quan hệ pháp luật.
D. Khả năng của các chủ thể được làm những gì mà pháp luật không cấm.

**Câu 62:** Khẳng định nào sau đây là đúng:
*A. Sự kiện pháp lý là sự cụ thể hoá phần giả định của Quy phạm pháp luật trong thực tiễn.
B. Sự kiện pháp lý là sự cụ thể hoá phần quy định của Quy phạm pháp luật trong thực tiễn.
C. Sự kiện pháp lý là sự cụ thể hoá phần chế tài của Quy phạm pháp luật trong thực tiễn.
D. Tất cả các đáp án đều sai.

**Câu 63:** Sự kiện pháp lý có thể:
A. Làm phát sinh một Quan hệ pháp luật cụ thể.
B. Làm thay đổi một Quan hệ pháp luật cụ thể.
C. Làm chấm dứt một Quan hệ pháp luật cụ thể.
*D. Tất cả các đáp án đều đúng.

**Câu 64:** Theo quy định của Pháp luật:
*A. Năng lực pháp luật của các chủ thể là giống nhau.
B. Năng lực pháp luật của các chủ thể là khác nhau.
C. Năng lực pháp luật của các chủ thể có thể giống nhau, có thể khác nhau, tùy theo từng trường hợp cụ thể.
D. Tất cả các đáp án đều sai.

## CHƯƠNG 4

**Câu 65:** Hình thức trách nhiệm nghiêm khắc nhất theo quy định của pháp luật Việt Nam:
A. Trách nhiệm hành chính.
*B. Trách nhiệm hình sự.
C. Trách nhiệm dân sự.
D. Trách nhiệm kỷ luật.

**Câu 66:** Các loại vi phạm pháp luật:
A. Vi phạm hình sự.
B. Vi phạm hình sự, vi phạm hành chính.
C. Vi phạm hình sự, vi phạm hành chính và vi phạm dân sự.
*D. Vi phạm hình sự, vi phạm hành chính, vi phạm dân sự và vi phạm kỉ luật.

**Câu 67:** Loại vi phạm pháp luật nào gây hậu quả lớn nhất cho xã hội:
*A. Vi phạm hình sự.
B. Vi phạm hành chính.
C. Vi phạm dân sự.
D. Vi phạm kỷ luật.

**Câu 68:** Khẳng định nào là đúng:
A. Mọi hành vi trái pháp luật là hành vi vi phạm pháp luật.
*B. Mọi hành vi vi phạm pháp luật là hành vi trái pháp luật.
C. Hành vi trái pháp luật có thể là hành vi vi phạm pháp luật, có thể không phải là hành vi vi phạm pháp luật.
D. Tất cả các đáp án đều sai.

**Câu 69:** Yếu tố nào sau đây không thể hiện nội dung mối quan hệ nhân quả giữa hành vi trái pháp luật và sự thiệt hại của xã hội:
A. Hành vi trái pháp luật là nguyên nhân trực tiếp.
B. Sự thiệt hại của xã hội là kết quả tất yếu.
C. Hậu quả của vi phạm pháp luật phù hợp với mục đích của chủ thể.
*D. Hành vi xảy ra trước sự thiệt hại.

## CHƯƠNG 5

**Câu 70:** Theo Hiến pháp Việt Nam 2013, Chủ tịch Quốc hội nước CHXHCN Việt Nam:
A. Do nhân dân bầu ra.
*B. Do Quốc hội bầu ra.
C. Do Chủ tịch nước chỉ định.
D. Do Đảng cộng sản bầu ra.

**Câu 71:** Theo Hiến pháp Việt Nam 2013, Chủ tịch Nước CHXHCN Việt Nam:
A. Do nhân dân bầu ra.
*B. Do Quốc hội bầu ra.
C. Do Chủ tịch quốc hội chỉ định.
D. Do Đảng cộng sản bầu ra.

**Câu 72:** Theo Hiến pháp Việt Nam 2013, Thủ tướng Chính phủ nước CHXHCN Việt Nam:
A. Do nhân dân bầu ra.
*B. Do Quốc hội bầu ra.
C. Do Chủ tịch nước chỉ định.
D. Do Đảng cộng sản bầu ra.

**Câu 73:** Theo Hiến pháp Việt Nam 2013, Chánh án Tòa án nhân dân tối cao nước CHXHCN Việt Nam:
A. Do nhân dân bầu ra.
*B. Do Quốc hội bầu ra.
C. Do Chủ tịch nước chỉ định.
D. Do Đảng cộng sản bầu ra.

**Câu 74:** Theo Hiến pháp Việt Nam 2013, Viện trưởng Viện kiểm sát nhân dân tối cao nước CHXHCN Việt Nam:
A. Do nhân dân bầu ra.
*B. Do Quốc hội bầu ra.
C. Do Chủ tịch nước chỉ định.
D. Do Đảng cộng sản bầu ra.

**Câu 75:** Theo Hiến pháp Việt Nam 2013, người được bầu vào chức danh Chủ tịch nước, Chủ tịch Quốc hội, Thủ tướng Chính phủ, Bộ trưởng, Thủ trưởng cơ quan ngang bộ, có nhiệm kỳ:
A. 3 năm.
B. 4 năm.
*C. 5 năm.
D. 6 năm.

**Câu 76:** Cơ quan hành chính có tên gọi là “Sở” là cơ quan nhà nước thuộc cấp nào:
A. Cấp trung ương.
*B. Cấp tỉnh.
C. Cấp huyện.
D. Tất cả các đáp án đều đúng.

**Câu 77:** Chế định “Chế độ chính trị” do ngành luật nào điều chỉnh?
*A. Ngành luật nhà nước (Ngành luật hiến pháp).
B. Ngành luật hành chính.
C. Ngành luật hình sự.
D. Ngành luật dân sự.

**Câu 78:** Chế định “Chế độ kinh tế” do ngành luật nào điều chỉnh:
A. Ngành luật kinh tế.
B. Ngành luật tài chính.
C. Ngành luật lao động.
*D. Ngành luật nhà nước (ngành luật hiến pháp).

**Câu 79:** Theo Hiến pháp Việt Nam 2013, Thủ tướng Chính phủ Nước CHXHCN Việt Nam:
A. Do Chủ tịch nước giới thiệu.
*B. Do Quốc hội bầu theo sự giới thiệu của Chủ tịch nước.
C. Do nhân dân bầu ra.
D. Do Đảng cộng sản bầu ra.

**Câu 80:** Bản Hiến pháp đang có hiệu lực của Nhà nước CHXHCN Việt Nam:
*A. Hiến pháp 2013.
B. Hiến pháp 2001.
C. Hiến pháp 1992.
D. Hiến pháp 1980.

**Câu 81:** Theo quy định Hiến pháp Việt Nam 2013, cơ quan nào sau đây có chức năng xét xử:
A. Chính phủ.
B. Quốc hội.
*C. Cơ quan Tòa án.
D. Viện kiểm sát nhân dân.

**Câu 82:** Chế định “Quyền và nghĩa vụ cơ bản của công dân” thuộc ngành luật nào:
A. Ngành luật hành chính.
B. Ngành luật hôn nhân và gia đình.
C. Ngành luật lao động.
*D. Ngành luật nhà nước (ngành luật hiến pháp).

**Câu 83:** Phương pháp điều chỉnh của ngành luật nhà nước (ngành luật hiến pháp):
A. Phương pháp định nghĩa.
B. Phương pháp bắt buộc.
C. Phương pháp quyền uy.
*D. Tất cả các đáp án đều đúng.

**Câu 84:** Nguồn của ngành luật nhà nước (ngành luật hiến pháp):
A. Hiến pháp là nguồn duy nhất của ngành luật nhà nước.
B. Ngoài Hiến pháp thì các đạo luật về tổ chức bộ máy nhà nước cũng là nguồn của ngành luật nhà nước.
*C. Ngoài Hiến pháp và các đạo luật về tổ chức bộ máy nhà nước thì các nghị quyết của ĐCS cũng là nguồn của ngành luật nhà nước.
D. Tất cả các đáp án đều sai.

**Câu 85:** Theo Hiến pháp Việt Nam 2013, Thủ tướng Chính phủ Nước CHXHCN Việt Nam:
A. Do nhân dân bầu.
*B. Do Quốc hội bầu theo sự giới thiệu của Chủ tịch nước.
C. Do Chủ tịch nước giới thiệu.
D. Do Chính phủ bầu.

**Câu 86:** Cơ quan nào sau đây không phải là cơ quan hành chính Nhà nước:
A. Bộ Khoa học công nghệ.
B. Bộ Nội vụ.
C. Bộ Tài nguyên môi trường.
*D. Tòa án nhân dân tối cao.

**Câu 87:** Cơ quan nào sau đây không phải là cơ quan hành chính Nhà nước:
A. Bộ Công thương.
B. Bộ Nông nghiệp và phát triển nông thôn.
C. Bộ Công an.
*D. Viện kiểm sát nhân dân tối cao.

**Câu 88:** Theo quy định của Hiến pháp Việt Nam 2013, cơ quan duy nhất có quyền lập hiến và lập pháp là cơ quan nào:
A. Chủ tịch nước.
*B. Quốc hội.
C. Chính phủ.
D. Tòa án nhân dân và Viện kiểm sát nhân dân.

**Câu 89:** Cơ quan nào sau đây có chức năng quản lý hành chính:
A. Các Bộ.
B. Chính phủ.
C. UBND các cấp.
*D. Tất cả các đáp án đều đúng.

**Câu 90:** Cơ quan thực hiện chức năng thực hành quyền công tố và kiểm sát các hoạt động tư pháp là:
A. Quốc hội.
B. Chính phủ.
C. Tòa án nhân dân.
*D. Viện kiểm sát nhân dân.

**Câu 91:** Chế định “Văn hóa, giáo dục, khoa học, công nghệ” thuộc ngành luật nào:
A. Luật hành chính.
B. Luật dân sự.
C. Luật quốc tế.
*D. Luật nhà nước (Luật hiến pháp).

**Câu 92:** Theo quy định của Hiến pháp Việt Nam 2013:
A. Quốc hội là cơ quan quyền lực nhà nước cao nhất, đại diện cho quyền lợi của nhân dân Thủ đô Hà Nội.
B. Quốc hội là cơ quan quyền lực nhà nước cao nhất, đại diện cho quyền lợi của nhân dân cả nước.
C. Quốc hội là cơ quan quyền lực nhà nước cao nhất, đại diện cho quyền lợi của nhân dân địa phương nơi đại biểu được bầu ra.
*D. Tất cả các đáp án đều đúng.

**Câu 93:** Theo quy định của Hiến pháp Việt Nam 2013 và Luật tổ chức Tòa án thì Tòa án nhân dân có mấy cấp?
A. 2 cấp.
*B. 3 cấp.
C. 4 cấp.
D. 5 cấp.

**Câu 94:** Theo Hiến pháp Việt Nam 2013, Chủ tịch nước Nước CHXHCN Việt Nam:
A. Do nhân dân bầu ra.
*B. Do Quốc hội bầu ra.
C. Do nhân dân bầu và Quốc hội phê chuẩn.
D. Do Chính phủ bầu ra.

**Câu 95:** Nếu không có kỳ họp bất thường, theo quy định của Hiến pháp Việt Nam 2013, mỗi năm Quốc hội Việt Nam có mấy kỳ họp:
A. 1 kỳ.
*B. 2 kỳ.
C. 3 kỳ.
D. Không có quy định phải triệu tập mấy kỳ họp.

**Câu 96:** Cơ quan nào sau đây có chức năng truy tố ai đó ra trước pháp luật:
*A. Cơ quan Viện kiểm sát.
B. Cơ quan cảnh sát nhân dân.
C. Cơ quan công an nhân dân.
D. Tòa án nhân dân các cấp.

**Câu 97:** Theo quy định pháp luật về bầu cử của Việt Nam, muốn tham gia ứng cử, ngoài các điều kiện khác, về độ tuổi được quy định:
A. Từ đủ 18 tuổi.
*B. Từ đủ 21 tuổi.
C. Không quy định độ tuổi chung mà quy định theo các dân tộc khác nhau.
D. Không quy định về độ tuổi cụ thể mà quy định theo giới tính.

**Câu 98:** Cơ quan nào sau đây có chức năng quản lý hành chính:
A. Tòa án nhân dân và viện kiểm sát nhân dân.
*B. Chính phủ.
C. Hội đồng nhân dân các cấp.
D. Trường Đại học Điện lực.

**Câu 99:** Theo quy định của Hiến pháp Việt Nam 2013, Hội đồng nhân dân là cơ quan quyền lực nhà nước ở địa phương:
*A. Đại diện cho quyền lợi nhân dân địa phương nơi được bầu ra.
B. Đại diện cho quyền lợi của nhân dân cả nước.
C. Đại diện cho quyền lợi của nhân dân cả nước và đại diện cho quyền lợi của nhân dân địa phương nơi được bầu ra.
D. Đại diện cho UBND địa phương.

## CHƯƠNG 6

**Câu 100:** Khái niệm “Hành pháp” tương đương với khái niệm nào:
*A. Hành chính.
B. Lập pháp.
C. Tư pháp.
D. Kiểm sát.

**Câu 101:** Khái niệm “Hành pháp” tương đương với khái niệm nào:
A. Lập pháp.
B. Tư pháp.
*C. Quản lý nhà nước.
D. Kiểm sát.

**Câu 102:** Chủ thể thực hiện hoạt động quản lý nhà nước:
*A. Cơ quan hành chính Nhà nước.
B. Cơ quan Nhà nước.
C. Tòa án nhân dân tối cao.
D. Viện kiểm sát nhân dân tối cao.

**Câu 103:** Phương pháp quyền uy – phục tùng là phương pháp điều chỉnh của ngành luật nào:
*A. Ngành luật hình sự.
B. Ngành luật dân sự.
C. Ngành luật hôn nhân – gia đình.
D. Ngành luật Nhà nước.

**Câu 104:** Khái niệm “Hành pháp” tương đương với khái niệm nào:
A. Lập pháp.
*B. Chấp hành và điều hành.
C. Tư pháp.
D. Kiểm sát.

**Câu 105:** Đối tượng điều chỉnh của ngành luật nhà nước (ngành luật hiến pháp), là những QHXH:
A. Liên quan đến nguồn gốc của quyền lực nhà nước, bản chất của nhà nước.
B. Liên quan đến nguyên tắc tổ chức và hoạt động của các cơ quan, các tổ chức, các cá nhân thực hiện quyền lực nhà nước.
C. Liên quan đến việc xác định mối quan hệ giữa nhà nước và công dân.
*D. Tất cả các đáp án đều đúng.

**Câu 106:** Độ tuổi mà cá nhân có thể phải chịu trách nhiệm hành chính là:
*A. Từ đủ 14 tuổi.
B. Từ đủ 16 tuổi.
C. Từ đủ 18 tuổi.
D. Từ đủ 21 tuổi.

**Câu 107:** Đối tượng điều chỉnh của ngành luật hành chính:
A. Những Quan hệ xã hội mang tính chất chấp hành và điều hành phát sinh trong hoạt động của các cơ quan quản lý nhà nước.
B. Những Quan hệ xã hội mang tính chất chấp hành và điều hành phát sinh trong hoạt động xây dựng, tổ chức công tác nội bộ của các cơ quan Nhà nước khác.
C. Những Quan hệ xã hội mang tính chất chấp hành và điều hành phát sinh trong hoạt động của các cơ quan Nhà nước khác hoặc các Tổ chức xã hội khi được nhà nước trao quyền thực hiện chức năng quản lý nhà nước.
*D. Tất cả các đáp án đều đúng.

**Câu 108:** Phương pháp điều chỉnh của ngành luật hành chính:
*A. Phương pháp mệnh lệnh đơn phương.
B. Phương pháp quyền uy – phục tùng.
C. Phương pháp bình đẳng, thỏa thuận.
D. Định nghĩa, bắt buộc, quyền uy.

**Câu 109:** Chủ thể của ngành luật hành chính:
A. Mọi CQNN, những người có chức vụ cũng như mọi cán bộ, công chức, viên chức.
B. Các tổ chức xã hội, cơ quan xã hội.
C. Công dân, người nước ngoài và người không quốc tịch cư trú làm ăn sinh sống lâu dài trên lãnh thổ Việt Nam.
*D. Tất cả các đáp án đều đúng.

**Câu 110:** Hành vi “gây rối trật tự công cộng” là:
*A. Hành vi vi phạm hành chính.
B. Hành vi vi phạm dân sự.
C. Hành vi vi phạm luật lao động.
D. Hành vi vi phạm kỷ luật.

**Câu 111:** Khẳng định nào sau đây là đúng:
*A. Chủ thể của pháp luật hành chính là các cơ quan, nhân viên nhà nước, công dân và các chủ thể khác.
B. Chủ thể của pháp luật hành chính chỉ là các cơ quan, nhân viên nhà nước.
C. Chủ thể của pháp luật hành chính là các cơ quan, nhân viên nhà nước, công dân và người nước ngoài.
D. Tất cả các đáp án đều sai.

## CHƯƠNG 7

**Câu 112:** Cơ quan nào có quyền xét xử tội phạm và tuyên bản án hình sự:
A. Tòa kinh tế.
B. Tòa hành chính.
C. Tòa dân sự.
*D. Tòa hình sự.

**Câu 113:** Tòa án nào có quyền xét xử tội phạm và tuyên bản án hình sự:
A. Tòa kinh tế.
*B. Tòa hình sự.
C. Tòa hành chính.
D. Tòa dân sự, tòa hành chính.

**Câu 114:** Tùy theo mức độ phạm tội, tội phạm hình sự được chia thành các loại:
A. Tội phạm nghiêm trọng; tội phạm rất nghiêm trọng.
B. Tội phạm ít nghiêm trọng; tội phạm nghiêm trọng; tội phạm rất nghiêm trọng.
C. Tội phạm nghiêm trọng; tội phạm rất nghiêm trọng; tội phạm đặc biệt nghiêm trọng.
*D. Tội phạm ít nghiêm trọng; tội phạm nghiêm trọng; tội phạm rất nghiêm trọng; tội phạm đặc biệt nghiêm trọng.

**Câu 115:** Chế định “Hình phạt” thuộc ngành luật nào:
A. Ngành luật lao động.
B. Ngành luật hành chính.
*C. Ngành luật hình sự.
D. Ngành luật dân sự sự.

**Câu 116:** Theo quy định của Bộ luật hình sự Việt Nam 2015, độ tuổi nhỏ nhất phải chịu trách nhiệm hình sự là:
A. Từ đủ 6 tuổi.
*B. Từ đủ 14 tuổi.
C. Từ đủ 16 tuổi.
D. Từ đủ 18 tuổi.

**Câu 117:** Chế định “Tội phạm” thuộc ngành luật nào:
*A. Ngành luật hình sự.
B. Ngành luật tố tụng hình sự.
C. Ngành luật dân sự.
D. Ngành luật tố tụng dân sự.

**Câu 118:** Đối tượng điều chỉnh của ngành luật hình sự:
A. Những Quan hệ xã hội phát sinh giữa nhà nước và chủ thể phạm tội khi chủ thể này thực hiện một hành vi mà nhà nước quy định là tội phạm.
B. Những Quan hệ xã hội phát sinh giữa nhà nước với tất cả các cá nhân công dân, người nước ngoài, người không quốc tịch.
C. Những Quan hệ xã hội phát sinh giữa nhà nước với tất cả các cơ quan, tổ chức, cá nhân công dân, người nước ngoài, người không quốc tịch.
*D. Tất cả các đáp án đều đúng.

**Câu 119:** Phương pháp điều chỉnh của luật hình sự:
A. Phương pháp bình đẳng thỏa thuận.
*B. Phương pháp quyền uy – phục tùng.
C. Kết hợp phương pháp quyền uy và phương pháp bình đẳng thỏa thuận.
D. Tất cả các đáp án đều sai.

**Câu 120:** Khẳng định nào sau đây là đúng:
A. Trách nhiệm hình sự chỉ áp dụng đối với cá nhân thực hiện hành vi phạm tội.
B. Trách nhiệm hình sự chỉ áp dụng đối với pháp nhân thương mại thực hiện hành vi phạm tội.
*C. Trách nhiệm hình sự vừa áp dụng đối với cá nhân, vừa áp dụng đối với pháp nhân thương mại có hành vi phạm tội.
D. Tất cả các đáp án đều sai.

**Câu 121:** Những dấu hiệu cơ bản của tội phạm:
A. Tính nguy hiểm cho xã hội; Tính phải chịu hình phạt.
B. Tính có lỗi của tội phạm; Tính trái pháp luật hình sự.
C. Tính nguy hiểm cho xã hội, Tính trái pháp luật hình sự.
*D. Tính nguy hiểm cho xã hội; Tính phải chịu hình phạt, Tính có lỗi của tội phạm; Tính trái pháp luật hình sự.

**Câu 122:** Tội phạm hình sự được chia thành:
A. 3 loại.
*B. 4 loại.
C. 5 loại.
D. 6 loại.

**Câu 123:** Số lượng các hình phạt trong trách nhiệm hình sự:
A. Có 10 hình phạt chính và 10 hình phạt bổ sung.
B. Có 9 hình phạt chính và 9 hình phạt bổ sung.
C. Có 8 hình phạt chính và 8 hình phạt bổ sung.
*D. Có 7 hình phạt chính và 7 hình phạt bổ sung.

**Câu 124:** Trong các hình phạt của trách nhiệm hình sự:
A. Phạt tiền là hình phạt chính.
B. Phạt tiền là hình phạt bổ sung.
*C. Phạt tiền vừa là hình thức xử phạt chính vừa là hình thức xử phạt bổ sung.
D. Tất cả các đáp án đều sai.

**Câu 125:** Trong các hình phạt của trách nhiệm hình sự:
A. Trục xuất là hình phạt chính
B. Trục xuất là hình phạt bổ sung
*C. Trục xuất vừa là hình thức xử phạt chính vừa là hình thức xử phạt bổ sung
D. Tất cả các đáp án đều sai

**Câu 126:** Hình phạt bổ sung trong Bộ Luật hình sự là:
A. Cải tạo không giam giữ.
B. Cảnh cáo.
C. Tù có thời hạn.
*D. Tịch thu tài sản.

**Câu 127:** Nguyên tắc áp dụng hình phạt chính và hình phạt bổ sung trong pháp luật hình sự là:
A. Có thể áp dụng một lúc nhiều hình phạt chính và nhiều hình phạt bổ sung.
B. Chỉ có thể áp dụng một lúc được nhiều hình phạt chính, và chỉ áp dụng được một hình phạt bổ sung.
C. Chỉ có thể áp dụng được một hình phạt chính và một hình phạt bổ sung.
*D. Chỉ có thể áp dụng được một hình phạt chính và áp dụng được nhiều hình phạt bổ sung.

**Câu 128:** Chế định “Xóa án tích” thuộc ngành luật nào:
A. Ngành luật đất đai.
B. Ngành luật quốc tế.
C. Ngành luật lao động.
*D. Ngành luật hình sự.

**Câu 129:** Khẳng định nào sau đây là đúng:
*A. Chỉ có ngành luật hình sự mới quy định tội phạm và hình phạt.
B. Chỉ có ngành luật dân sự mới quy định tội phạm và hình phạt.
C. Cả ngành luật hình sự và ngành luật dân sự đều quy định tội phạm và hình phạt.
D. Tất cả các đáp án đều sai.

**Câu 131:** Tội phạm hình sự được chia thành:
A. Tội phạm nghiêm trọng và tội phạm đặc biệt nghiêm trọng.
B. Tội phạm ít nghiêm trọng và tội phạm nghiêm trọng.
C. Tội phạm ít nghiêm trọng, tội phạm nghiêm trọng và tội phạm rất nghiêm trọng.
*D. Tội phạm ít nghiêm trọng, tội phạm nghiêm trọng, tội phạm rất nghiêm trọng và tội phạm đặc biệt nghiêm trọng.

**Câu 132:** Khung hình phạt tương ứng với các mức độ tội phạm:
*A. Tội phạm ít nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 3 năm.
B. Tội phạm nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 3 năm.
C. Tội phạm rất nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 3 năm.
D. Tội phạm đặc biệt nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 3 năm.

**Câu 133:** Khung hình phạt tương ứng với các mức độ tội phạm:
A. Tội phạm ít nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 7 năm.
*B. Tội phạm nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 7 năm.
C. Tội phạm rất nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 7 năm.
D. Tội phạm đặc biệt nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 7 năm.

**Câu 134:** Khung hình phạt tương ứng với các mức độ tội phạm:
A. Tội phạm ít nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 15 năm.
B. Tội phạm nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 15 năm.
*C. Tội phạm rất nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 15 năm.
D. Tội phạm đặc biệt nghiêm trọng có khung hình phạt nhỏ hơn hoặc bằng 15 năm.

**Câu 135:** Khung hình phạt tương ứng với các mức độ tội phạm:
A. Tội phạm ít nghiêm trọng có khung hình phạt trên 15 năm tù, chung thân hoặc tử hình.
B. Tội phạm nghiêm trọng có khung hình phạt trên 15 năm tù, chung thân hoặc tử hình.
C. Tội phạm rất nghiêm trọng có khung hình phạt trên 15 năm tù, chung thân hoặc tử hình.
*D. Tội phạm đặc biệt nghiêm trọng có khung hình phạt trên 15 năm tù, chung thân hoặc tử hình.

**Câu 136:** Độ tuổi chịu trách nhiệm hình sự là:
A. Người từ đủ 12 tuổi phải chịu trách nhiệm hình sự về mọi tội phạm.
B. Người từ đủ 14 tuổi phải chịu trách nhiệm hình sự về mọi tội phạm.
*C. Người từ đủ 16 tuổi phải chịu trách nhiệm hình sự về mọi tội phạm.
D. Người từ đủ 18 tuổi phải chịu trách nhiệm hình sự về mọi tội phạm.

**Câu 137:** Trong trách nhiệm hình sự, xét về độ tuổi:
A. Người từ đủ 12 tuổi đến dưới 14 tuổi chỉ phải chịu trách nhiệm hình sự về tội phạm rất nghiêm trọng do cố ý hoặc tội phạm đặc biệt nghiêm trọng.
*B. Người từ đủ 14 tuổi đến dưới 16 tuổi chỉ phải chịu trách nhiệm hình sự về tội phạm rất nghiêm trọng do cố ý hoặc tội phạm đặc biệt nghiêm trọng.
C. Người từ đủ 16 tuổi đến dưới 18 tuổi chỉ phải chịu trách nhiệm hình sự về tội phạm rất nghiêm trọng do cố ý hoặc tội phạm đặc biệt nghiêm trọng.
D. Tất cả các đáp án trên đều sai.

**Câu 138:** Khẳng định nào sau đây là đúng:
*A. Trách nhiệm hình sự là dạng trách nhiệm pháp lý nghiêm khắc nhất.
B. Trách nhiệm hành chính là dạng trách nhiệm pháp lý nghiêm khắc nhất.
C. Trách nhiệm dân sự là dạng trách nhiệm pháp lý nghiêm khắc nhất.
D. Trách nhiệm kỷ luật và trách nhiệm vật chất là dạng trách nhiệm pháp lý nghiêm khắc nhất.

**Câu 139:** Khẳng định nào sau đây là đúng:
A. Mọi hành vi trái pháp luật hình sự được coi là tội phạm.
*B. Mọi tội phạm đều đã thực hiện hành vi trái pháp luật hình sự.
C. Mọi hành vi gây thiệt hại cho xã hội và có lỗi đều bị coi là tội phạm.
D. Mọi hành vi trái pháp luật hình sự được coi là vi phạm phá luật hình sự.

**Câu 140:** Chế định “Xóa án tích” thuộc ngành luật nào:
A. Ngành luật đất đai.
B. Ngành luật quốc tế.
C. Ngành luật lao động.
*D. Ngành luật hình sự.

**Câu 141:** Hình phạt tịch thu tài sản:
A. Là hình phạt chính.
*B. Là hình phạt bổ sung.
C. Vừa là hình phạt chính, vừa là hình phạt bổ sung.
D. Tất cả các đáp án trên đều sai.

**Câu 142:** Khẳng định nào sau đây là đúng:
*A. Quốc hội có thẩm quyền quy định tội phạm và hình phạt trong Bộ luật hình sự.
B. Tòa án nhân dân tối cao có thẩm quyền quy định tội phạm và hình phạt.
C. Chính phủ có thẩm quyền quy định tội phạm và hình phạt.
D. Tất cả các đáp án đều đúng.

**Câu 143:** Khẳng định nào sau đây là đúng:
*A. Chỉ có Tòa án có thẩm quyền mới được quyền áp dụng hình phạt đối với tội phạm.
B. Ngoài Tòa án thì Chính phủ cũng có quyền áp dụng hình phạt đối với tội phạm.
C. Ngoài Tòa án, Chính phủ thì Viện kiểm sát cũng có quyền áp dụng hình phạt đối với tội phạm.
D. Ngoài tòa án, Chính phủ, Viện kiểm sát thì Quốc hội cũng có quyền áp dụng hình phạt đối với tội phạm.

## CHƯƠNG 8

**Câu 144:** Xét về độ tuổi, người có Năng lực hành vi dân sự chưa đầy đủ khi:
A. Dưới 18 tuổi.
*B. Từ đủ 6 tuổi đến dưới 18 tuổi.
C. Từ đủ 15 tuổi đến dưới 18 tuổi.
D. Dưới 21 tuổi.

**Câu 145:** Xét về độ tuổi, người không có Năng lực hành vi dân sự khi:
A. Dưới 18 tuổi.
B. Từ đủ 6 tuổi đến dưới 18 tuổi.
C. Từ đủ 15 tuổi đến dưới 18 tuổi.
*D. Từ 0 tuổi đến dưới 6 tuổi.

**Câu 146:** Chế định “Giao dịch dân sự” thuộc ngành luật nào:
A. Ngành luật kinh tế.
B. Ngành luật hình sự.
C. Ngành luật hành chính.
*D. Ngành luật dân sự.

**Câu 147:** Chế định “Tài sản và quyền sở hữu” thuộc ngành luật nào:
A. Ngành luật kinh tế.
B. Ngành luật hôn nhân và gia đình.
C. Ngành luật lao động.
*D. Ngành luật dân sự.

**Câu 148:** Người nghiện ma túy hoặc các chất kích khác bị hạn chế Năng lực hành vi dân sự, khi:
A. Bị công an hạn chế Năng lực hành vi dân sự.
*B. Bị tòa án tuyên bố hạn chế Năng lực hành vi dân sự.
C. Bị viện kiểm sát hạn chế Năng lực hành vi dân sự.
D. Tất cả các đáp án đều đúng.

**Câu 149:** Chế định “Thừa kế” thuộc ngành luật nào:
A. Ngành luật hành chính.
B. Ngành luật tố tụng hình sự.
C. Ngành luật quốc tế.
*D. Ngành luật dân sự.

**Câu 150:** Tài sản theo ngành luật dân sự bao gồm:
A. Vật; Tiền.
B. Giấy tờ có giá; Các quyền tài sản.
*C. Vật; tiền, giấy tờ có giá; các quyền tài sản.
D. Tất cả các đáp án đều sai.

**Câu 151:** Trong các quan hệ dân sự:
*A. Các bên có sự bình đẳng về địa vị pháp lý.
B. Các bên không có sự bình đẳng về địa vị pháp lý.
C. Tùy từng trường hợp mà các bên có thể bình đẳng hoặc không bình đẳng về địa vị pháp lý.
D. Tất cả các đáp án đều sai.

**Câu 152:** Cá nhân trong ngành luật dân sự gồm:
A. Người VN.
B. Người nước ngoài.
C. Người không quốc tịch.
*D. Tất cả các đáp án đều đúng.

**Câu 153:** Đối tượng điều chỉnh của ngành luật dân sự:
A. Chỉ bao gồm các quan hệ tài sản.
B. Chỉ bao gồm các quan hệ nhân thân.
*C. Các quan hệ tài sản và các quan hệ nhân thân.
D. Tất cả các đáp án đều sai.

**Câu 154:** Xét về độ tuổi, người có Năng lực hành vi dân sự đầy đủ:
A. Từ đủ 16 tuổi.
*B. Từ đủ 18 tuổi.
C. Từ đủ 21 tuổi.
D. Từ đủ 25 tuổi.

**Câu 155:** Người bị mất Năng lực hành vi dân sự là người do bị bệnh tâm thần hoặc mắc bệnh khác:
A. Mà không thể nhận thức, làm chủ được hành vi của mình.
B. Mà không thể nhận thức, làm chủ được hành vi của mình thì theo yêu cầu của người có quyền, lợi ích liên quan, Tòa án ra quyết định tuyên bố mất Năng lực hành vi dân sự kể cả khi chưa có kết luận của tổ chức giám định.
*C. Mà không thể nhận thức, làm chủ được hành vi của mình thì theo yêu cầu của người có quyền, lợi ích liên quan, Tòa án ra quyết định tuyên bố mất Năng lực hành vi dân sự trên cơ sở kết luận của tổ chức giám định.
D. Tất cả các đáp án trên đều đúng.

**Câu 156:** Chế định “Pháp nhân” thuộc ngành luật nào:
*A. Ngành luật dân sự.
B. Ngành luật tố tụng dân sự.
C. Ngành luật hôn nhân và gia đình.
D. Ngành luật lao động.

**Câu 157:** Một cá nhân bị coi là mất Năng lực hành vi dân sự khi:
A. Họ nghiện ma túy hoặc các chất kích thích khác dẫn đến phát tán tài sản của gia đình, theo yêu cầu của những người có quyền và nghĩa vụ liên quan Tòa sẽ tuyên.
B. Từ đủ 6 tuổi đến dưới 18 tuổi.
*C. Họ bị bệnh tâm thần hoặc các bệnh khác không nhận thức và làm chủ hành vi của mình theo yêu cầu của những người có quyền và nghĩa vụ liên quan Tòa án sẽ tuyên.
D. Họ từ 0 đến dưới 6 tuổi.

**Câu 158:** Một cá nhân bị coi là hạn chế Năng lực hành vi dân sự khi:
*A. Họ nghiện ma túy hoặc các chất kích thích khác dẫn đến phát tán tài sản của gia đình, theo yêu cầu của những người có quyền và nghĩa vụ liên quan Tòa sẽ tuyên họ bị hạn chế Năng lực hành vi.
B. Từ đủ 6 tuổi đến dưới 18 tuổi.
C. Họ bị bệnh tâm thần hoặc các bệnh khác không nhận thức và làm chủ hành vi của mình theo yêu cầu của những người có quyền và nghĩa vụ liên quan Tòa án sẽ tuyên.
D. Họ từ 0 đến dưới 6 tuổi.

**Câu 159:** Phương pháp điều chỉnh của ngành luật dân sự có đặc điểm:
A. Bảo đảm sự bình đẳng về mặt pháp lý giữa các chủ thể.
B. Bảo đảm quyền tự định đoạt của các chủ thể.
C. Truy cứu trách nhiệm tài sản của những người có hành vi gây thiệt hại cho người khác nếu có đủ điều kiện quy định về việc bồi thường thiệt hại.
*D. Tất cả các đáp án đều đúng.

**Câu 160:** Chủ thể của ngành luật dân sự bao gồm:
A. Cá nhân, pháp nhân.
B. Cá nhân, pháp nhân, tổ hợp tác.
C. Cá nhân.
*D. Cá nhân, pháp nhân, tổ hợp tác, hộ gia đình.

**Câu 161:** Người lập di chúc chưa chết thì có thể hủy bỏ di chúc do mình lập ra hay không, nếu nó đã được trao cho người thừa kế:
*A. Được hủy bỏ.
B. Không được hủy bỏ.
C. Có thể hủy bỏ nếu những người thừa kế thỏa thuận được với nhau.
D. Có thể hủy bỏ nếu được cơ quan nhà nước có thẩm quyền cho phép.

**Câu 162:** Trong các vụ án hình sự:
A. Không bao giờ liên quan đến phần dân sự.
*B. Có thể liên quan đến phần dân sự.
C. Luôn liên quan đến phần dân sự.
D. Tất cả các đáp án đều sai.
`;

      // STATE & VARIABLES
      let questions = [];
      let currentView = "dashboard";
      let quizState = {
        questions: [],
        currentIndex: 0,
        score: 0,
        isFinished: false,
        timer: null,
        timeRemaining: 0,
        startTime: 0,
      };

      // 1. DATA PARSING
      function parseQuestions(text) {
        const lines = text.split("\n");
        const result = [];
        let currentChapter = "";
        let currentQ = null;

        lines.forEach((line) => {
          line = line.trim();
          if (!line) return;

          // Check for Chapter
          if (line.startsWith("##")) {
            currentChapter = line.replace("##", "").trim();
            return;
          }

          // Check for Question start
          const questionMatch = line.match(/^\*\*Câu\s+(\d+):\*\*(.*)/);
          if (questionMatch) {
            if (currentQ) result.push(currentQ);
            currentQ = {
              id: parseInt(questionMatch[1]),
              chapter: currentChapter,
              text: questionMatch[2].trim(),
              options: [],
              correctIndex: -1,
            };
            return;
          }

          // Check for Options
          // Matches "A. Content" or "*A. Content"
          const optionMatch = line.match(/^(\*)?([A-D])\.\s+(.*)/);
          if (optionMatch && currentQ) {
            const isCorrect = !!optionMatch[1];
            const letter = optionMatch[2]; // A, B, C, D
            const content = optionMatch[3];

            currentQ.options.push({
              letter: letter,
              content: content,
            });

            if (isCorrect) {
              currentQ.correctIndex = currentQ.options.length - 1;
            }
          }
        });
        if (currentQ) result.push(currentQ);
        return result;
      }

      // 2. INITIALIZATION
      window.addEventListener("load", () => {
        setTimeout(() => {
          questions = parseQuestions(rawMarkdown);
          updateStats();
          initChart();
          renderPracticeList(questions);

          // Hide loader, show dashboard
          document.getElementById("loading").classList.add("hidden");
          document.getElementById("view-dashboard").classList.remove("hidden");
        }, 500); // Fake small delay for smoother UX
      });

      // 3. NAVIGATION
      function switchView(viewName) {
        // Hide all views
        [
          "dashboard",
          "practice",
          "quiz-setup",
          "quiz-active",
          "quiz-result",
          "ai-chat",
        ].forEach((id) => {
          document.getElementById(`view-${id}`).classList.add("hidden");
        });

        // Show selected
        document.getElementById(`view-${viewName}`).classList.remove("hidden");

        // Update Nav State
        const navIds = [
          "nav-dashboard",
          "nav-practice",
          "nav-quiz",
          "nav-ai-chat",
        ];
        navIds.forEach((id) => {
          const el = document.getElementById(id);
          if (id === `nav-${viewName.split("-")[0]}`) {
            // Highlight active
            if (id === "nav-ai-chat") {
              el.classList.add("bg-purple-100", "text-purple-800");
            } else {
              el.classList.add("bg-amber-100", "text-amber-800");
            }
          } else {
            // Reset inactive
            el.classList.remove(
              "bg-amber-100",
              "text-amber-800",
              "bg-purple-100",
              "text-purple-800",
            );
          }
        });

        currentView = viewName;
        window.scrollTo(0, 0);
      }

      // 4. DASHBOARD LOGIC
      function updateStats() {
        document.getElementById("stat-total-q").innerText = questions.length;
        const chapters = [...new Set(questions.map((q) => q.chapter))];
        document.getElementById("stat-total-c").innerText = chapters.length;
      }

      function initChart() {
        const chapterCounts = {};
        questions.forEach((q) => {
          chapterCounts[q.chapter] = (chapterCounts[q.chapter] || 0) + 1;
        });

        const ctx = document.getElementById("chapterChart").getContext("2d");
        new Chart(ctx, {
          type: "bar",
          data: {
            labels: Object.keys(chapterCounts),
            datasets: [
              {
                label: "Số câu hỏi",
                data: Object.values(chapterCounts),
                backgroundColor: "#d97706",
                borderRadius: 6,
              },
            ],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
              legend: { display: false },
            },
            scales: {
              y: { beginAtZero: true },
            },
          },
        });
      }

      // 5. PRACTICE MODE LOGIC
      const searchInput = document.getElementById("searchInput");
      searchInput.addEventListener("input", (e) => {
        const term = e.target.value.toLowerCase();
        const filtered = questions.filter(
          (q) =>
            q.text.toLowerCase().includes(term) ||
            q.chapter.toLowerCase().includes(term),
        );
        renderPracticeList(filtered);
      });

      function renderPracticeList(data) {
        const container = document.getElementById("questions-list");
        const noResults = document.getElementById("no-results");
        container.innerHTML = "";

        if (data.length === 0) {
          noResults.classList.remove("hidden");
          return;
        }
        noResults.classList.add("hidden");

        let lastChapter = "";

        data.forEach((q) => {
          // Chapter Header grouping
          if (q.chapter !== lastChapter) {
            const h = document.createElement("h3");
            h.className =
              "text-lg font-bold text-amber-800 mt-6 mb-2 border-b border-amber-200 pb-1";
            h.innerText = q.chapter;
            container.appendChild(h);
            lastChapter = q.chapter;
          }

          // Question Card
          const card = document.createElement("div");
          card.className =
            "bg-white p-4 rounded-lg border border-gray-200 hover:border-amber-300 transition shadow-sm";

          // Toggle Answer logic
          const correctOpt = q.options[q.correctIndex];

          // Unique ID for AI explanation
          const aiDivId = `ai-explain-${q.id}`;

          card.innerHTML = `
                    <div class="flex justify-between items-start mb-2">
                        <span class="text-xs font-bold text-gray-400">Câu ${q.id}</span>
                        <div class="space-x-2">
                             <button onclick="document.getElementById('ans-${q.id}').classList.toggle('hidden')" class="text-xs font-bold text-blue-600 hover:text-blue-800 bg-blue-50 px-2 py-1 rounded transition">👁️ Xem đáp án</button>
                             <button onclick="askAIExplain(${q.id})" class="text-xs font-bold text-purple-600 hover:text-purple-800 bg-purple-50 px-2 py-1 rounded transition">✨ Giải thích chi tiết</button>
                        </div>
                    </div>
                    <div id="ans-${q.id}" class="hidden mb-3 p-2 bg-green-50 text-green-800 text-sm rounded border border-green-100 animate-fade-in">
                        ✅ Đáp án đúng: <b>${correctOpt.letter}. ${correctOpt.content}</b>
                    </div>
                    <div id="${aiDivId}" class="hidden mb-3 p-3 bg-purple-50 text-gray-800 text-sm rounded border border-purple-100 prose prose-sm max-w-none">
                        <!-- AI Content goes here -->
                    </div>
                    <p class="font-medium text-gray-800 mb-3">${q.text}</p>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-2 text-sm text-gray-600">
                        ${q.options.map((o) => `<div class="${o === correctOpt ? "font-semibold text-green-700" : ""}">${o.letter}. ${o.content}</div>`).join("")}
                    </div>
                `;
          container.appendChild(card);
        });
      }

      // 6. QUIZ LOGIC
      function setQuickConfig(count, time) {
        document.getElementById("setup-count").value = count;
        document.getElementById("setup-count-display").innerText =
          (count === 162 ? "Tất cả" : count) + " câu";
        document.getElementById("setup-time").value = time;
      }

      function startCustomQuiz() {
        const count = parseInt(document.getElementById("setup-count").value);
        const time = parseInt(document.getElementById("setup-time").value);
        startQuiz("custom", count, time);
      }

      function startQuiz(mode, count, timeLimit) {
        let quizQs = [];

        // Logic selection question
        if (count >= questions.length || count === 162) {
          // Use all questions shuffled
          quizQs = [...questions].sort(() => 0.5 - Math.random());
        } else {
          // Randomize
          const shuffled = [...questions].sort(() => 0.5 - Math.random());
          quizQs = shuffled.slice(0, count);
        }

        quizState = {
          questions: quizQs,
          currentIndex: 0,
          score: 0,
          isFinished: false,
          startTime: Date.now(),
          timeRemaining: timeLimit * 60, // in seconds
          timer: null,
        };

        switchView("quiz-active");

        // Setup Timer
        const timerContainer = document.getElementById("timer-container");
        const timerDisplay = document.getElementById("quiz-timer");

        if (timeLimit > 0) {
          timerContainer.classList.remove("hidden");
          updateTimerDisplay();
          quizState.timer = setInterval(() => {
            quizState.timeRemaining--;
            updateTimerDisplay();
            if (quizState.timeRemaining <= 0) {
              endQuiz(true);
            }
          }, 1000);
        } else {
          timerContainer.classList.add("hidden");
        }

        renderQuizQuestion();
        updateQuizProgress();
      }

      function updateTimerDisplay() {
        const mins = Math.floor(quizState.timeRemaining / 60);
        const secs = quizState.timeRemaining % 60;
        const display = `${mins.toString().padStart(2, "0")}:${secs.toString().padStart(2, "0")}`;
        const el = document.getElementById("quiz-timer");
        el.innerText = display;

        // Visual warning
        if (quizState.timeRemaining < 60) {
          el.classList.add("text-red-600", "animate-pulse");
        } else {
          el.classList.remove("text-red-600", "animate-pulse");
        }
      }

      function renderQuizQuestion() {
        const q = quizState.questions[quizState.currentIndex];

        // DOM Elements
        document.getElementById("quiz-chapter-tag").innerText = q.chapter;
        document.getElementById("quiz-question-text").innerText =
          `Câu ${q.id}: ${q.text}`;

        const optionsContainer = document.getElementById(
          "quiz-options-container",
        );
        optionsContainer.innerHTML = "";

        // Hide feedback
        document.getElementById("quiz-feedback").classList.add("hidden");

        // Render Options
        q.options.forEach((opt, index) => {
          const btn = document.createElement("button");
          btn.className =
            "btn-option w-full text-left p-4 rounded-xl border-2 border-gray-100 hover:border-amber-300 hover:bg-amber-50 focus:outline-none relative group";
          btn.onclick = () => handleAnswer(index, btn);

          btn.innerHTML = `
                    <span class="font-bold text-gray-400 mr-2 group-hover:text-amber-600">${opt.letter}.</span>
                    <span class="text-gray-700 font-medium">${opt.content}</span>
                `;
          optionsContainer.appendChild(btn);
        });
      }

      function handleAnswer(selectedIndex, btnElement) {
        if (document.querySelector(".answered")) return; // Prevent double click

        const q = quizState.questions[quizState.currentIndex];
        const isCorrect = selectedIndex === q.correctIndex;
        const feedbackArea = document.getElementById("quiz-feedback");
        const feedbackText = document.getElementById("quiz-feedback-text");

        // Mark all buttons as answered state
        const buttons = document.querySelectorAll(".btn-option");
        buttons.forEach((b) => {
          b.classList.add("answered", "cursor-default");
          b.classList.remove("hover:border-amber-300", "hover:bg-amber-50");
          b.disabled = true;
        });

        // Highlight chosen and correct
        if (isCorrect) {
          btnElement.classList.add("bg-green-100", "border-green-500");
          quizState.score++;
          feedbackText.innerText = "🎉 Chính xác!";
          feedbackText.className = "font-bold mb-2 text-green-600";
        } else {
          btnElement.classList.add("bg-red-100", "border-red-500");
          // Highlight real correct answer
          buttons[q.correctIndex].classList.add(
            "bg-green-100",
            "border-green-500",
          );

          feedbackText.innerText =
            "❌ Sai rồi! Đáp án đúng là " + q.options[q.correctIndex].letter;
          feedbackText.className = "font-bold mb-2 text-red-600";
        }

        // Update Score UI
        updateQuizProgress();

        // Show Next Button
        feedbackArea.classList.remove("hidden");
      }

      function nextQuestion() {
        quizState.currentIndex++;
        if (quizState.currentIndex < quizState.questions.length) {
          renderQuizQuestion();
          updateQuizProgress();
        } else {
          endQuiz();
        }
      }

      function updateQuizProgress() {
        const current = quizState.currentIndex + 1;
        const total = quizState.questions.length;
        document.getElementById("quiz-current").innerText = current;
        document.getElementById("quiz-total").innerText = total;
        document.getElementById("quiz-score").innerText = quizState.score;

        // Update bar
        const pct = ((current - 1) / total) * 100;
        document.getElementById("quiz-progress-bar").style.width = `${pct}%`;
      }

      function endQuiz(isTimeout = false) {
        clearInterval(quizState.timer);
        switchView("quiz-result");

        const total = quizState.questions.length;
        const score = quizState.score;
        const percent = Math.round((score / total) * 100);

        // Calculate Time Taken
        const timeTakenSec = Math.floor(
          (Date.now() - quizState.startTime) / 1000,
        );
        const mins = Math.floor(timeTakenSec / 60);
        const secs = timeTakenSec % 60;

        // Set Text
        document.getElementById("result-score").innerText = score;
        document.getElementById("result-total").innerText = total;
        document.getElementById("result-percent").innerText = `${percent}%`;
        document.getElementById("result-time").innerText =
          `${mins.toString().padStart(2, "0")}:${secs.toString().padStart(2, "0")}`;
        document.getElementById("result-answered").innerText = isTimeout
          ? quizState.currentIndex
          : total;

        // Determine Rank
        let title = "";
        let emoji = "";
        let sub = "";

        if (percent === 100) {
          emoji = "👑";
          title = "THẦN PHÁP LUẬT";
          sub = "Không thể tin được! Bạn đã đạt điểm tuyệt đối!";
        } else if (percent >= 80) {
          emoji = "⚖️";
          title = "LUẬT SƯ TÀI BA";
          sub = "Kiến thức của bạn rất vững chắc. Tuyệt vời!";
        } else if (percent >= 65) {
          emoji = "🎓";
          title = "CỬ NHÂN LUẬT";
          sub = "Kết quả tốt, nhưng vẫn còn chỗ để cải thiện.";
        } else if (percent >= 50) {
          emoji = "📚";
          title = "SINH VIÊN LUẬT";
          sub = "Bạn đã vượt qua mức trung bình. Cố lên!";
        } else {
          emoji = "🐣";
          title = "TẬP SỰ";
          sub = "Hãy ôn tập thêm trong phần Tra Cứu nhé.";
        }

        document.getElementById("result-emoji").innerText = emoji;
        document.getElementById("result-title").innerText = title;
        document.getElementById("result-subtitle").innerText = isTimeout
          ? "Hết giờ làm bài! " + sub
          : sub;
      }

      // ============================================
      // 7. GEMINI AI INTEGRATION
      // ============================================

      async function callGeminiAPI(prompt) {
        const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;

        try {
          const response = await fetch(url, {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              contents: [
                {
                  parts: [{ text: prompt }],
                },
              ],
            }),
          });

          if (!response.ok) {
            throw new Error(`API Error: ${response.status}`);
          }

          const data = await response.json();
          return data.candidates[0].content.parts[0].text;
        } catch (error) {
          console.error("Gemini Error:", error);
          return "Xin lỗi, hiện tại hệ thống AI đang bận hoặc gặp lỗi. Vui lòng thử lại sau.";
        }
      }

      // Feature 1: Contextual Explanation
      async function askAIExplain(questionId) {
        const container = document.getElementById(`ai-explain-${questionId}`);

        // Toggle visibility
        if (!container.classList.contains("hidden")) {
          container.classList.add("hidden");
          return;
        }
        container.classList.remove("hidden");

        // If already loaded, don't recall API
        if (container.dataset.loaded === "true") return;

        // Show loading
        container.innerHTML =
          '<span class="animate-pulse">✨ AI đang phân tích luật pháp...</span>';

        const q = questions.find((item) => item.id === questionId);
        const correctOpt = q.options[q.correctIndex];
        const optionsText = q.options
          .map((o) => `${o.letter}. ${o.content}`)
          .join("\n");

        const prompt = `Bạn là một giảng viên luật uy tín tại Việt Nam. Hãy giải thích tại sao đáp án "${correctOpt.letter}" là đúng cho câu hỏi sau đây:\n\n"${q.text}"\n\nCác lựa chọn:\n${optionsText}\n\nHãy giải thích ngắn gọn, súc tích dựa trên lý thuyết pháp luật hoặc Hiến pháp Việt Nam.`;

        const result = await callGeminiAPI(prompt);

        // Render Markdown using Marked.js
        container.innerHTML = marked.parse(result);
        container.dataset.loaded = "true";
      }

      // Feature 2: AI Chat Assistant
      async function sendChatMessage() {
        const input = document.getElementById("chat-input");
        const history = document.getElementById("chat-history");
        const userText = input.value.trim();
        const btn = document.getElementById("btn-send-chat");

        if (!userText) return;

        // User UI
        const userBubble = document.createElement("div");
        userBubble.className = "flex justify-end";
        userBubble.innerHTML = `<div class="bg-purple-600 text-white rounded-2xl rounded-tr-none px-4 py-3 max-w-[80%]">${userText}</div>`;
        history.appendChild(userBubble);

        input.value = "";
        history.scrollTop = history.scrollHeight;

        // Disable button
        btn.disabled = true;
        btn.innerHTML =
          '<span class="animate-spin h-5 w-5 border-2 border-white border-t-transparent rounded-full mr-2"></span>';

        // Loading UI
        const loadingBubble = document.createElement("div");
        loadingBubble.id = "ai-loading-bubble";
        loadingBubble.className = "flex justify-start";
        loadingBubble.innerHTML = `<div class="bg-gray-100 rounded-2xl rounded-tl-none px-4 py-3 max-w-[80%] text-gray-500 italic">Đang suy nghĩ...</div>`;
        history.appendChild(loadingBubble);
        history.scrollTop = history.scrollHeight;

        // Call API
        const prompt = `Bạn là Trợ lý Luật sư AI hữu ích, am hiểu pháp luật Việt Nam. Hãy trả lời câu hỏi sau của người dùng một cách chính xác, thân thiện và dễ hiểu:\n\n"${userText}"`;

        const aiResponse = await callGeminiAPI(prompt);

        // Remove loading
        document.getElementById("ai-loading-bubble").remove();

        // AI UI
        const aiBubble = document.createElement("div");
        aiBubble.className = "flex justify-start";
        aiBubble.innerHTML = `<div class="bg-gray-100 rounded-2xl rounded-tl-none px-4 py-3 max-w-[80%] text-gray-800 prose prose-sm">${marked.parse(aiResponse)}</div>`;
        history.appendChild(aiBubble);

        history.scrollTop = history.scrollHeight;
        btn.disabled = false;
        btn.innerHTML = "<span>Gửi</span>";
      }
    </script>
  </body>
</html>
