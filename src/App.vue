<template>
  <div class="flex min-h-screen flex-col bg-spot-bg font-sans text-spot-navy antialiased">
    <!-- Toast Message Alert System -->
    <div
      v-if="toast.visible"
      class="fixed top-20 right-5 z-50 flex items-center gap-3 px-5 py-3 rounded-xl shadow-xl bg-spot-navy text-white border border-slate-700 transition-all duration-300 transform translate-y-0 text-xs font-semibold animate-bounce"
    >
      <i class="fas fa-info-circle text-spot-coral"></i>
      <span>{{ toast.message }}</span>
    </div>

    <!-- GNB: White GNB with Navy Text -->
    <header
      class="sticky top-0 z-40 bg-spot-gnb border-b border-spot-border shadow-xs px-4 lg:px-8 h-16 flex items-center justify-between"
    >
      <div class="flex items-center gap-2 cursor-pointer" @click="switchTab('home')">
        <div
          class="w-7 h-7 rounded bg-spot-navy flex items-center justify-center text-white text-xs font-black"
        >
          <i class="fas fa-map-pin"></i>
        </div>
        <span class="text-base font-extrabold text-spot-navy tracking-tight flex flex-col">
          <span class="leading-none text-xs text-spot-coral tracking-widest font-black uppercase"
            >SpotSeoul</span
          >
          <span class="text-[9px] text-slate-400 font-bold">스팟서울</span>
        </span>
      </div>

      <!-- Desktop Navigation Items with Active Capsule Pill Layout -->
      <nav class="hidden md:flex items-center gap-1.5 text-xs font-bold text-spot-navy">
        <button
          v-for="tab in tabItems"
          :key="tab.id"
          @click="switchTab(tab.id)"
          :class="[
            'px-3.5 py-1.5 rounded-lg transition-all duration-200',
            activeTab === tab.id
              ? 'bg-spot-navy text-white shadow-sm'
              : 'hover:bg-spot-border/50 text-spot-navy/80',
          ]"
        >
          {{ tab.name }}
        </button>
      </nav>

      <button
        @click="mobileMenuOpen = !mobileMenuOpen"
        class="md:hidden p-2 rounded-lg bg-slate-100 text-spot-navy hover:bg-slate-200 transition-all"
        aria-label="메뉴 열기"
      >
        <i :class="mobileMenuOpen ? 'fas fa-times' : 'fas fa-bars'"></i>
      </button>
    </header>

    <div v-if="mobileMenuOpen" class="md:hidden bg-white border-b border-spot-border shadow-sm">
      <nav class="flex flex-col gap-2 px-4 py-3 text-xs font-bold text-spot-navy">
        <button
          v-for="tab in tabItems"
          :key="tab.id"
          @click="switchTab(tab.id)"
          :class="[
            'w-full text-left px-3 py-2 rounded-lg transition-all duration-200',
            activeTab === tab.id
              ? 'bg-spot-navy text-white'
              : 'hover:bg-spot-border/50 text-spot-navy/80',
          ]"
        >
          {{ tab.name }}
        </button>
      </nav>
    </div>

    <!-- MAIN VIEW AREA -->
    <main class="flex-grow flex flex-col">
      <!-- ==================== VIEW 1: HOME PAGE ==================== -->
      <div v-if="activeTab === 'home'" class="flex-grow flex flex-col">
        <!-- Hero Section -->
        <section
          class="relative h-[420px] bg-slate-900 flex items-center justify-center text-center px-4 overflow-hidden"
        >
          <div
            class="absolute inset-0 bg-cover bg-center"
            style="background-image: url('/images/img_home_bg.jpg')"
          ></div>
          <div class="absolute inset-0 bg-black/50"></div>
          <div class="relative z-10 max-w-2xl space-y-4">
            <span
              class="px-3 py-1 bg-white/10 rounded-full text-[10px] font-bold text-white tracking-widest uppercase border border-white/20"
              >SEOUL, SOUTH KOREA</span
            >
            <h1 class="text-3xl sm:text-5xl font-black text-white tracking-tight leading-tight">
              서울을 탐색하세요
            </h1>
            <p class="text-sm sm:text-base text-slate-200 font-medium">
              관광지 · 맛집 · 숙소 · 축제 반드시 가봐야 할 명소를 한 곳에서
            </p>
            <div class="flex justify-center gap-3 pt-2">
              <button
                @click="switchTab('spots')"
                class="px-6 py-3 bg-spot-coral hover:bg-spot-coral/95 text-white font-bold text-xs sm:text-sm rounded-lg shadow-lg transition-all transform hover:scale-102"
              >
                명소 둘러보기
              </button>
              <button
                @click="switchTab('chatbot')"
                class="px-6 py-3 bg-white/10 hover:bg-white/20 border border-white/30 text-white font-bold text-xs sm:text-sm rounded-lg transition-all"
              >
                AI 챗봇 이용
              </button>
            </div>
          </div>
        </section>

        <!-- Categories Section -->
        <section class="max-w-6xl mx-auto px-6 py-12 w-full space-y-6">
          <div class="text-center space-y-1">
            <span class="text-[10px] text-spot-coral font-bold tracking-widest uppercase block"
              >DISCOVER SEOUL</span
            >
            <h2 class="text-xl sm:text-2xl font-extrabold text-spot-navy">무엇을 찾고 계신가요?</h2>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-6 gap-4">
            <div
              v-for="cat in homeCategories"
              :key="cat.name"
              @click="goToSpotsWithCategory(cat.name)"
              class="group cursor-pointer overflow-hidden rounded-2xl border border-spot-border bg-white text-left transition-all hover:-translate-y-1 hover:border-spot-coral/50 hover:shadow-lg"
            >
              <!-- 카테고리 이미지 -->
              <div class="h-28 w-full overflow-hidden bg-slate-100">
                <img
                  :src="cat.image"
                  :alt="cat.name"
                  class="h-full w-full object-cover transition-transform duration-300 group-hover:scale-105"
                />
              </div>

              <!-- 카드 내용 -->
              <div class="p-4">
                <div
                  :class="[
                    'mb-3 flex h-9 w-9 items-center justify-center rounded-xl text-sm text-white',
                    cat.color,
                  ]"
                >
                  <i :class="cat.icon"></i>
                </div>

                <h4
                  class="text-sm font-bold text-spot-navy transition-colors group-hover:text-spot-coral"
                >
                  {{ cat.name }}
                </h4>

                <p class="mt-1 text-[10px] text-slate-400">
                  {{ cat.desc }}
                </p>

                <span class="mt-3 inline-block text-[10px] font-bold text-spot-coral">
                  탐색 <i class="fas fa-chevron-right ml-1"></i>
                </span>
              </div>
            </div>
          </div>
        </section>

        <!-- Recommended Places (Deep Navy section) -->
        <section class="bg-spot-navy text-white py-14 px-6">
          <div class="max-w-6xl mx-auto space-y-8">
            <div class="flex items-end justify-between border-b border-slate-800 pb-4">
              <div>
                <span class="text-[9px] text-spot-coral font-bold tracking-wider block"
                  >EDITOR'S PICKS</span
                >
                <h2 class="text-xl sm:text-2xl font-black">추천 명소</h2>
              </div>
              <button
                @click="switchTab('spots')"
                class="text-xs text-slate-400 hover:text-white transition-all"
              >
                전체보기 <i class="fas fa-chevron-right text-[10px]"></i>
              </button>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-4 gap-5">
              <div
                v-for="spot in places.slice(0, 4)"
                :key="spot.id"
                @click="openSpotDetail(spot)"
                class="bg-slate-900/60 border border-slate-800 rounded-2xl overflow-hidden cursor-pointer hover:border-slate-600 hover:scale-102 transition-all"
              >
                <div
                  class="h-40 bg-cover bg-center relative"
                  :style="`background-image: url('${spot.img}');`"
                ></div>
                <div class="p-4 space-y-1.5">
                  <span
                    class="text-[9px] bg-spot-coral/20 text-spot-coral font-bold px-1.5 py-0.5 rounded"
                    >{{ spot.category }}</span
                  >
                  <h4 class="font-extrabold text-xs sm:text-sm text-slate-100 truncate mt-1">
                    {{ spot.name }}
                  </h4>
                  <p class="text-[10px] text-slate-400 truncate">
                    <i class="fas fa-map-marker-alt text-spot-coral mr-1"></i>{{ spot.address }}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </section>

        <!-- Traveler Board List Preview -->
        <section class="max-w-6xl mx-auto px-6 py-12 w-full space-y-6">
          <div class="flex items-end justify-between border-b border-spot-border pb-4">
            <div>
              <span class="text-[9px] text-spot-coral font-bold tracking-wider block"
                >COMMUNITY</span
              >
              <h2 class="text-xl sm:text-2xl font-black text-spot-navy">여행자 게시판</h2>
            </div>
            <button
              @click="switchTab('board')"
              class="text-xs text-spot-coral hover:underline transition-all"
            >
              전체보기 <i class="fas fa-chevron-right text-[10px]"></i>
            </button>
          </div>

          <div class="space-y-3">
            <div
              v-for="post in posts.slice(0, 4)"
              :key="post.id"
              @click="goToPostDetail(post)"
              class="bg-white border border-spot-border rounded-xl p-4 flex items-center justify-between cursor-pointer hover:border-spot-coral/50 hover:shadow-xs transition-all"
            >
              <div class="flex items-center gap-3">
                <span
                  :class="[
                    'text-[10px] font-black px-2 py-1 rounded w-16 text-center',
                    getBadgeClass(post.category),
                  ]"
                >
                  {{ post.category }}
                </span>
                <span
                  class="font-bold text-xs sm:text-sm text-spot-navy truncate hover:text-spot-coral"
                  >{{ post.title }}</span
                >
              </div>
              <div
                class="flex items-center gap-4 text-[10px] text-slate-400 font-semibold whitespace-nowrap"
              >
                <span><i class="far fa-eye mr-1"></i>{{ post.views }}</span>
                <span>{{ timeAgo(post.created_at) }}</span>
              </div>
            </div>
          </div>
        </section>
      </div>

      <!-- ==================== VIEW 2: SPOTS VIEW ==================== -->
      <div
        v-if="activeTab === 'spots'"
        class="max-w-6xl mx-auto px-6 py-8 w-full space-y-6 flex-grow flex flex-col"
      >
        <div class="space-y-1">
          <span class="text-[9px] text-spot-coral font-bold tracking-widest uppercase block"
            >DISCOVER</span
          >
          <h2 class="text-2xl font-extrabold text-spot-navy">서울 명소</h2>
          <p class="text-xs text-slate-500 font-medium">
            관광지 · 문화시설 · 레포츠 · 쇼핑 · 축제 · 숙소
            | 서울에서 꼭 방문해야할 명품 장소들입니다.
          </p>
        </div>

        <!-- Filters Section with Search functionality -->
        <div
          class="flex flex-col sm:flex-row items-center justify-between gap-3 bg-white p-3 rounded-xl border border-spot-border"
        >
          <div class="flex flex-wrap gap-1.5 w-full sm:w-auto">
            <button
              v-for="cat in ['전체', '관광지', '문화시설', '레포츠', '쇼핑', '축제', '숙박']"
              :key="cat"
              @click="selectSpotCategory(cat)"
              :class="[
                'px-3 py-1.5 rounded-lg text-xs font-bold transition-all',
                selectedCategory === cat
                  ? 'bg-spot-navy text-white shadow-xs'
                  : 'bg-spot-bg text-spot-navy hover:bg-spot-border/50',
              ]"
            >
              {{ cat }}
            </button>
          </div>
          <div class="relative w-full sm:w-72">
            <i class="fas fa-search absolute left-3 top-2.5 text-slate-400 text-xs"></i>
            <input
              type="text"
              v-model="spotSearchQuery"
              @input="searchSpots"
              placeholder="명소, 주소, 태그 검색..."
              class="w-full pl-8 pr-4 py-2 bg-spot-bg border border-spot-border focus:border-spot-navy focus:outline-none rounded-lg text-xs"
            />
          </div>
        </div>

        <!-- Skeleton Loader -->
        <div v-if="loading" class="grid grid-cols-1 sm:grid-cols-4 gap-6 flex-grow">
          <div
            v-for="n in 8"
            :key="n"
            class="bg-white border border-spot-border rounded-2xl overflow-hidden p-4 space-y-3 animate-pulse"
          >
            <div class="h-40 bg-slate-200 rounded-xl"></div>
            <div class="h-4 bg-slate-200 w-2/3 rounded"></div>
            <div class="h-3 bg-slate-200 w-1/2 rounded"></div>
          </div>
        </div>

        <!-- Spots Grid -->
        <div v-else class="grid grid-cols-1 sm:grid-cols-4 gap-6">
          <div
            v-for="spot in places"
            :key="spot.id"
            @click="openSpotDetail(spot)"
            class="bg-white border border-spot-border rounded-2xl overflow-hidden cursor-pointer hover:border-spot-coral/50 hover:shadow-md transition-all flex flex-col h-full"
          >
            <div
              class="h-40 bg-cover bg-center relative"
              :style="`background-image: url('${spot.img}');`"
            ></div>
            <div class="p-4 flex-grow flex flex-col justify-between space-y-2">
              <div class="space-y-1">
                <span
                  class="text-[9px] bg-slate-100 text-slate-600 font-bold px-1.5 py-0.5 rounded border border-slate-200"
                  >{{ spot.category }}</span
                >
                <h4 class="font-bold text-xs sm:text-sm text-spot-navy truncate mt-1.5">
                  {{ spot.name }}
                </h4>
                <p class="text-[10px] text-slate-400 line-clamp-2 leading-relaxed">
                  {{ spot.desc }}
                </p>
              </div>
              <div
                class="border-t border-slate-100 pt-2 flex items-center justify-between text-[9px] text-slate-400"
              >
                <span class="truncate"
                  ><i class="fas fa-map-marker-alt text-spot-coral mr-1"></i
                  >{{ spot.address }}</span
                >
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- ==================== VIEW 3: MAP VIEW ==================== -->
      <div v-show="activeTab === 'map'" class="flex-grow flex h-[calc(100vh-4rem)] overflow-hidden">
        <!-- Sidebar list panel -->
        <div class="w-80 bg-white border-r border-spot-border flex flex-col h-full overflow-y-auto">
          <div class="p-4 border-b border-spot-border space-y-3">
            <h3 class="font-extrabold text-sm text-spot-navy">서울 장소 탐색</h3>
            <div class="relative">
              <i class="fas fa-search absolute left-3 top-2.5 text-slate-400 text-xs"></i>
              <input
                type="text"
                v-model="mapSearchQuery"
                @input="filterMapPlaces"
                placeholder="장소명으로 찾기..."
                class="w-full pl-8 pr-4 py-2 bg-spot-bg border border-spot-border rounded-lg text-xs focus:outline-none"
              />
            </div>
            <div class="flex gap-1 overflow-x-auto pb-1">
              <button
                v-for="cat in ['전체', '관광지', '문화시설', '레포츠', '쇼핑', '축제']"
                :key="cat"
                @click="selectMapCategory(cat)"
                :class="[
                  'px-2 py-1 rounded text-[9px] font-bold whitespace-nowrap',
                  mapFilterCategory === cat
                    ? 'bg-spot-navy text-white'
                    : 'bg-spot-bg text-slate-600',
                ]"
              >
                {{ cat }}
              </button>
            </div>
          </div>

          <div class="divide-y divide-spot-border flex-grow overflow-y-auto">
            <div
              v-for="spot in mapPlaces"
              :key="spot.id"
              @click="focusMapOn(spot)"
              :class="[
                'p-3 flex gap-3 cursor-pointer hover:bg-spot-bg/40 transition-colors',
                selectedMapSpotId === spot.id ? 'bg-spot-bg' : '',
              ]"
            >
              <div
                class="w-12 h-12 rounded bg-cover bg-center flex-shrink-0"
                :style="`background-image: url('${spot.img}');`"
              ></div>
              <div class="min-w-0 flex-grow space-y-1">
                <span
                  class="text-[8px] bg-slate-100 border text-slate-500 px-1 py-0.2 rounded font-bold"
                  >{{ spot.category }}</span
                >
                <h4 class="font-bold text-xs text-spot-navy truncate">{{ spot.name }}</h4>
                <p class="text-[9px] text-slate-400 truncate">{{ spot.address }}</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Leaflet Container -->
        <div class="flex-grow h-full bg-slate-200 relative" id="map-container"></div>
      </div>

      <!-- ==================== VIEW 4: COURSE VIEW ==================== -->
      <div
        v-if="activeTab === 'courses'"
        class="max-w-6xl mx-auto px-6 py-8 w-full space-y-6 flex-grow flex flex-col"
      >
        <div class="space-y-1">
          <span class="text-[9px] text-spot-coral font-bold tracking-widest uppercase block"
            >TRAVEL ITINERARY</span
          >
          <h2 class="text-2xl font-extrabold text-spot-navy">여행 코스</h2>
          <p class="text-xs text-slate-500 font-medium">
            서울 여행지 제목과 위치를 기반으로 묶인 추천 일정을 확인하세요.
          </p>
        </div>

        <div class="grid gap-3">
          <div class="bg-white p-4 rounded-3xl border border-spot-border">
            <div class="grid gap-4">
              <!-- Theme Group -->
              <div>
                <div class="text-sm font-bold text-spot-navy mb-2">테마</div>
                <div class="flex flex-wrap gap-2 lg:gap-3 items-center">
                  <div class="flex gap-2 overflow-x-auto w-full lg:overflow-visible lg:w-auto py-1">
                    <button
                      v-for="theme in courseThemeOptions"
                      :key="theme.value"
                      @click="selectCourseTheme(theme.value)"
                      :class="[
                        'flex-shrink-0 px-4 py-2 rounded-full text-sm font-semibold transition-all whitespace-nowrap',
                        selectedCourseTheme === theme.value
                          ? 'bg-spot-navy text-white border border-spot-navy'
                          : 'bg-white text-spot-navy border border-spot-border hover:bg-slate-50'
                      ]"
                    >
                      {{ theme.label }}
                    </button>
                  </div>
                </div>
              </div>

              <!-- Duration Group -->
              <div>
                <div class="text-sm font-bold text-spot-navy mb-2">일정</div>
                <div class="flex gap-2 items-center">
                  <div class="flex gap-2 overflow-x-auto w-full lg:overflow-visible lg:w-auto py-1">
                    <button
                      v-for="duration in courseDurationOptions"
                      :key="duration.value"
                      @click="selectCourseDuration(duration.value)"
                      :class="[
                        'flex-shrink-0 px-4 py-2 rounded-full text-sm font-semibold transition-all whitespace-nowrap',
                        selectedCourseDuration === duration.value
                          ? 'bg-spot-navy text-white border border-spot-navy'
                          : 'bg-white text-spot-navy border border-spot-border hover:bg-slate-50'
                      ]"
                    >
                      {{ duration.label }}
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
          <div
            v-for="course in courseGroups"
            :key="course.id"
            @click="openCourseDetail(course)"
            class="group bg-white border border-spot-border rounded-3xl overflow-hidden shadow-xs hover:shadow-lg transition-all cursor-pointer"
          >
            <div class="relative h-56 bg-slate-200 overflow-hidden">
              <img
                v-if="course.representative_image"
                :src="course.representative_image"
                alt="대표 이미지"
                class="h-full w-full object-cover transition duration-500 group-hover:scale-105"
              />
              <div class="absolute inset-0 bg-gradient-to-t from-slate-950/80 to-transparent"></div>
              <div class="absolute left-4 top-4 flex flex-wrap gap-2">
                <span class="rounded-full bg-spot-coral px-3 py-1 text-[10px] font-bold text-white">{{ course.theme_label }}</span>
                <span class="rounded-full bg-white/90 px-3 py-1 text-[10px] font-bold text-slate-900">{{ course.duration_label }}</span>
              </div>
            </div>
            <div class="bg-white p-4">
              <h3 class="text-lg font-black text-spot-navy leading-tight">{{ course.title }}</h3>
              <p class="mt-2 text-sm text-slate-500 line-clamp-3">{{ course.summary }}</p>
            </div>
          </div>
        </div>
      </div>

      <div v-if="courseDetail" @click.self="closeCourseDetail" class="fixed inset-0 z-50 flex items-center justify-center bg-slate-950/70 px-4 py-6 overflow-y-auto">
        <div class="w-full max-w-xl md:max-w-3xl lg:max-w-5xl bg-white rounded-[20px] shadow-2xl max-h-[90vh] overflow-hidden">
          <div class="relative md:h-72 h-48 bg-slate-900">
            <img
              v-if="courseDetail.representative_image"
              :src="courseDetail.representative_image"
              alt="코스 대표 이미지"
              class="absolute inset-0 h-full w-full object-cover"
            />
            <div class="absolute inset-0 bg-gradient-to-t from-slate-950/90 to-transparent"></div>
            <button
              @click="closeCourseDetail"
              class="absolute right-4 top-4 rounded-full bg-white/90 text-slate-900 w-10 h-10 flex items-center justify-center shadow"
            >
              <i class="fas fa-times"></i>
            </button>
            <div class="absolute bottom-6 left-6 text-white max-w-2xl">
              <p class="text-[10px] uppercase tracking-[0.28em] text-slate-300">{{ courseDetail.theme_label }} · {{ courseDetail.duration_label }}</p>
              <h3 class="mt-3 text-3xl font-black leading-tight">{{ courseDetail.title }}</h3>
              <p class="mt-3 text-sm text-white/80">{{ courseDetail.summary }}</p>
            </div>
          </div>

          <div class="p-6 space-y-6 text-slate-700 overflow-auto max-h-[55vh] md:max-h-[60vh]">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-sm">
              <div class="rounded-3xl bg-spot-bg p-4">
                <p class="text-[10px] text-slate-500 uppercase tracking-[0.2em]">포함 여행지</p>
                <p class="mt-2 text-lg font-bold">{{ courseDetail.item_count }}곳</p>
              </div>
              <div class="rounded-3xl bg-spot-bg p-4">
                <p class="text-[10px] text-slate-500 uppercase tracking-[0.2em]">이동거리</p>
                <p class="mt-2 text-lg font-bold">{{ courseDetail.span_km }} km</p>
              </div>
              <!-- Removed 수정일 card as requested -->
            </div>
            <div class="grid gap-4">
              <div
                v-for="stop in courseDetail.items"
                :key="stop.contentid"
                class="grid gap-4 md:grid-cols-[120px_1fr] rounded-3xl border border-spot-border overflow-hidden"
              >
                <div class="h-32 bg-slate-200 overflow-hidden">
                  <img
                    v-if="stop.firstimage || stop.firstimage2"
                    :src="stop.firstimage || stop.firstimage2"
                    :alt="stop.title"
                    class="h-full w-full object-cover"
                  />
                </div>
                <div class="p-4">
                  <div class="flex items-center justify-between gap-3">
                    <h4 class="text-base font-bold text-spot-navy">{{ stop.title }}</h4>
                    <span class="text-[10px] text-slate-500">{{ stop.cat2 || stop.cat3 || '분류 없음' }}</span>
                  </div>
                  <p class="mt-2 text-[12px] text-slate-500">{{ stop.addr1 || stop.addr2 || '주소 정보 없음' }}</p>
                  <p class="mt-1 text-[12px] text-slate-400">전화: {{ stop.tel || '없음' }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- ==================== VIEW 5: BOARD VIEW ==================== -->
      <div
        v-if="activeTab === 'board'"
        class="max-w-4xl mx-auto px-6 py-8 w-full space-y-6 flex-grow flex flex-col"
      >
        <!-- LIST SUB-VIEW -->
        <div v-if="boardSubView === 'list'" class="space-y-4 flex-grow flex flex-col">
          <div class="flex items-center justify-between border-b border-spot-border pb-4">
            <div class="space-y-1">
              <span class="text-[9px] text-spot-coral font-bold tracking-widest uppercase block"
                >COMMUNITY</span
              >
              <h2 class="text-2xl font-extrabold text-spot-navy">여행자 게시판</h2>
              <p class="text-xs text-slate-500 font-medium emoji-text">
                서울 여행자들과 소통하세요
              </p>
            </div>
            <button
              @click="openBoardWrite"
              class="px-4 py-2 bg-spot-navy text-white hover:bg-slate-800 text-xs font-bold rounded-lg flex items-center gap-1.5 shadow transition-all"
            >
              <i class="fas fa-plus"></i> 글쓰기
            </button>
          </div>

          <div
            class="flex flex-col sm:flex-row items-center justify-between gap-2.5 bg-white p-3 rounded-xl border border-spot-border"
          >
            <div class="flex flex-wrap gap-1">
              <button
                v-for="tag in [
                  '전체',
                  '관광지',
                  '문화시설',
                  '레포츠',
                  '쇼핑',
                  '축제',
                  '맛집',
                  '숙박',
                  '기타',
                ]"
                :key="tag"
                @click="selectBoardCategory(tag)"
                :class="[
                  'px-2.5 py-1 rounded text-xs font-bold transition-all',
                  selectedBoardCategory === tag
                    ? 'bg-spot-navy text-white'
                    : 'bg-spot-bg text-spot-navy hover:bg-spot-border/50',
                ]"
              >
                {{ tag }}
              </button>
            </div>
            <div class="relative w-full sm:w-60">
              <i class="fas fa-search absolute left-3 top-2.5 text-slate-400 text-xs"></i>
              <input
                type="text"
                v-model="boardSearchQuery"
                @input="fetchPosts"
                placeholder="게시글 내용 검색..."
                class="w-full pl-8 pr-4 py-2 bg-spot-bg border border-spot-border rounded-lg text-xs focus:outline-none"
              />
            </div>
          </div>

          <div v-if="posts.length > 0" class="space-y-2.5">
            <div
              v-for="post in posts"
              :key="post.id"
              @click="goToPostDetail(post)"
              class="bg-white border border-spot-border rounded-xl p-4 flex items-center justify-between cursor-pointer hover:border-spot-coral/50 transition-all"
            >
              <div class="flex items-center gap-3">
                <span
                  :class="[
                    'text-[10px] font-black px-2 py-1 rounded w-16 text-center',
                    getBadgeClass(post.category),
                  ]"
                >
                  {{ post.category }}
                </span>
                <span
                  class="font-bold text-xs sm:text-sm text-spot-navy truncate hover:text-spot-coral"
                  >{{ post.title }}</span
                >
              </div>
              <div
                class="flex items-center gap-4 text-[10px] text-slate-400 font-bold whitespace-nowrap"
              >
                <span><i class="far fa-eye mr-1"></i>{{ post.views }}</span>
                <span>{{ timeAgo(post.created_at) }}</span>
              </div>
            </div>
          </div>
          <div
            v-else
            class="text-center py-16 bg-white border border-spot-border rounded-2xl text-slate-400 text-xs"
          >
            <i class="fas fa-edit mb-2 block text-lg"></i>
            해당 검색 기준에 매칭되는 익명 공유글이 없습니다.
          </div>
        </div>

        <!-- DETAIL SUB-VIEW -->
        <div v-if="boardSubView === 'detail' && currentPost" class="space-y-5 flex-grow">
          <div class="flex items-center justify-between border-b border-spot-border pb-3">
            <button
              @click="backToBoardList"
              class="text-xs text-slate-500 hover:text-spot-navy flex items-center gap-1.5 font-bold"
            >
              <i class="fas fa-arrow-left"></i> 목록으로
            </button>
            <div class="flex gap-2">
              <button
                @click="openPasswordModal('edit')"
                class="px-2.5 py-1.5 bg-white border border-spot-border rounded-lg text-xs font-bold hover:bg-slate-50 text-spot-navy"
              >
                수정
              </button>
              <button
                @click="openPasswordModal('delete')"
                class="px-2.5 py-1.5 bg-rose-50 text-rose-600 rounded-lg text-xs font-bold hover:bg-rose-100"
              >
                삭제
              </button>
            </div>
          </div>

          <article class="bg-white border border-spot-border rounded-2xl p-6 space-y-4">
            <div class="border-b border-slate-100 pb-3">
              <div class="flex items-center gap-2 text-[10px] sm:text-xs text-slate-400 mb-1.5">
                <span
                  :class="['font-black px-2 py-0.5 rounded', getBadgeClass(currentPost.category)]"
                  >{{ currentPost.category }}</span
                >
                <span>•</span>
                <span>{{ currentPost.author }}</span>
                <span>•</span>
                <span>조회수 {{ currentPost.views }}</span>
              </div>
              <h3 class="font-extrabold text-base sm:text-lg text-spot-navy mt-1">
                {{ currentPost.title }}
              </h3>
            </div>
            <p class="text-slate-600 text-xs sm:text-sm leading-relaxed whitespace-pre-wrap">
              {{ currentPost.content }}
            </p>
          </article>

          <!-- Comments Section -->
          <div class="bg-white border border-spot-border rounded-2xl p-5 space-y-4">
            <h4 class="font-extrabold text-xs sm:text-sm">
              댓글 피드백 ({{ currentPost.comments ? currentPost.comments.length : 0 }}개)
            </h4>

            <div
              v-if="currentPost.comments && currentPost.comments.length > 0"
              class="divide-y divide-slate-100"
            >
              <div v-for="com in currentPost.comments" :key="com.id" class="py-3 text-xs space-y-2">
                <div class="flex items-center justify-between gap-2">
                  <span class="font-black text-spot-navy">{{ com.author }}</span>
                  <div class="flex items-center gap-2 text-[9px] text-slate-400">
                    <button
                      @click="openCommentPasswordModal('edit', com.id)"
                      class="font-bold text-spot-navy hover:text-slate-800"
                    >
                      수정
                    </button>
                    <button
                      @click="openCommentPasswordModal('delete', com.id)"
                      class="font-bold text-rose-600 hover:text-rose-800"
                    >
                      삭제
                    </button>
                    <span>{{ timeAgo(com.created_at) }}</span>
                  </div>
                </div>
                <p class="text-slate-600 leading-relaxed">{{ com.content }}</p>
              </div>
            </div>
            <p v-else class="text-center py-4 text-slate-400 text-xs">
              작성된 댓글 소통이 없습니다. 첫 마디를 달아보세요!
            </p>

            <!-- Add/Edit Comment Form -->
            <form @submit.prevent="submitComment" class="space-y-2 pt-2 border-t border-spot-border">
              <div class="grid grid-cols-2 gap-2">
                <input
                  type="text"
                  v-model="commentForm.author"
                  placeholder="닉네임 (익명)"
                  required
                  class="w-full px-3 py-1.5 border border-spot-border rounded-lg text-xs focus:outline-none bg-spot-bg"
                />
                <input
                  type="password"
                  v-model="commentForm.password"
                  placeholder="수정 비번(숫자 4자리)"
                  maxlength="4"
                  required
                  class="w-full px-3 py-1.5 border border-spot-border rounded-lg text-xs focus:outline-none bg-spot-bg"
                />
              </div>
              <div class="flex gap-2">
                <input
                  type="text"
                  v-model="commentForm.content"
                  placeholder="댓글 소통을 시도해보세요..."
                  required
                  class="flex-grow px-3 py-2 border border-spot-border rounded-lg text-xs focus:outline-none focus:ring-1 focus:ring-spot-navy bg-spot-bg"
                />
                <button
                  type="submit"
                  class="px-4 py-2 bg-spot-navy text-white text-xs font-bold rounded-lg hover:bg-slate-800 transition-colors"
                >
                  {{ commentEditId ? '수정' : '등록' }}
                </button>
              </div>
            </form>
          </div>
        </div>

        <!-- WRITE SUB-VIEW -->
        <div
          v-if="boardSubView === 'write'"
          class="bg-white border border-spot-border rounded-2xl p-6 space-y-4"
        >
          <div class="flex items-center justify-between border-b border-spot-border pb-3">
            <h3 class="font-extrabold text-sm sm:text-base text-spot-navy">새 정보 공유하기</h3>
            <button
              @click="cancelWritePost"
              class="text-xs text-slate-400 hover:text-slate-600 font-bold"
            >
              취소
            </button>
          </div>

          <form @submit.prevent="createPost" class="space-y-4">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
              <div>
                <label class="block text-[10px] font-bold text-slate-500 mb-1">분류</label>
                <select
                  v-model="writeForm.category"
                  class="w-full px-3 py-2 border border-spot-border bg-spot-bg rounded-lg text-xs focus:outline-none"
                >
                  <option
                    v-for="cat in [
                      '관광지',
                      '문화시설',
                      '레포츠',
                      '쇼핑',
                      '축제',
                      '맛집',
                      '숙박',
                      '기타',
                    ]"
                    :key="cat"
                    :value="cat"
                  >
                    {{ cat }}
                  </option>
                </select>
              </div>
              <div>
                <label class="block text-[10px] font-bold text-slate-500 mb-1">작성자</label>
                <input
                  type="text"
                  v-model="writeForm.author"
                  required
                  placeholder="익명"
                  class="w-full px-3 py-2 border border-spot-border bg-spot-bg rounded-lg text-xs focus:outline-none"
                />
              </div>
              <div>
                <label class="block text-[10px] font-bold text-slate-500 mb-1"
                  >비밀번호 (4자리)</label
                >
                <input
                  type="password"
                  v-model="writeForm.password"
                  required
                  placeholder="숫자 4자리"
                  maxlength="4"
                  class="w-full px-3 py-2 border border-spot-border bg-spot-bg rounded-lg text-xs focus:outline-none"
                />
              </div>
            </div>

            <div>
              <label class="block text-[10px] font-bold text-slate-500 mb-1">제목</label>
              <input
                type="text"
                v-model="writeForm.title"
                required
                placeholder="공유할 서울 여행 꿀팁의 핵심 제목"
                class="w-full px-3.5 py-2.5 border border-spot-border bg-spot-bg rounded-lg text-xs focus:outline-none"
              />
            </div>

            <div>
              <label class="block text-[10px] font-bold text-slate-500 mb-1">내용</label>
              <textarea
                v-model="writeForm.content"
                required
                rows="6"
                placeholder="다른 이용자들에게 소개하고 싶은 유용한 지역 로컬 정보를 솔직하게 들려주세요."
                class="w-full px-3.5 py-2.5 border border-spot-border bg-spot-bg rounded-lg text-xs focus:outline-none resize-none"
              ></textarea>
            </div>

            <div class="flex items-center justify-end gap-2 pt-2 border-t border-slate-100">
              <button
                type="button"
                @click="boardSubView = 'list'"
                class="px-4 py-2 border border-spot-border text-slate-600 rounded-lg text-xs font-bold hover:bg-slate-50"
              >
                취소
              </button>
              <button
                type="submit"
                class="px-5 py-2 bg-spot-navy hover:bg-slate-800 text-white rounded-lg text-xs font-extrabold shadow"
              >
                발행하기
              </button>
            </div>
          </form>
        </div>
      </div>

      <!-- ==================== VIEW 6: CHATBOT VIEW ==================== -->
      <div
        v-show="activeTab === 'chatbot'"
        class="max-w-4xl mx-auto px-6 py-8 w-full flex-grow flex flex-col h-[calc(100vh-4rem)] overflow-hidden"
      >
        <div
          class="flex items-center justify-between border-b border-spot-border pb-3 flex-shrink-0"
        >
          <div class="flex items-center gap-2.5">
            <div
              class="w-9 h-9 rounded-lg bg-spot-navy text-white flex items-center justify-center text-sm"
            >
              <i class="fas fa-robot"></i>
            </div>
            <div>
              <span class="text-[9px] text-spot-coral font-bold uppercase tracking-wider block"
                >AI ASSISTANT</span
              >
              <h2 class="text-base sm:text-lg font-black text-spot-navy leading-none mt-0.5">
                SpotSeoul 챗봇
              </h2>
            </div>
          </div>
          <span
            class="text-[10px] bg-emerald-50 text-emerald-700 font-extrabold px-2.5 py-1 rounded-full border border-emerald-100 flex items-center gap-1"
          >
            <span class="w-1.5 h-1.5 bg-emerald-500 rounded-full animate-ping"></span> • 온라인
            (FastAPI 서버 연동)
          </span>
        </div>
        
        <!-- Chat Message Feed -->
        <div class="mb-4 bg-white/90 border border-spot-border rounded-[32px] shadow-[0_30px_60px_-40px_rgba(15,23,42,0.38)] p-4">
          <div class="flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
            <div class="flex items-center gap-3">
              <div class="relative">
                <img
                  src="/images/spoti2.png"
                  alt="SpotSeoul 챗봇 캐릭터"
                  class="w-20 h-20 rounded-[28px] border-4 border-white shadow-lg object-cover"
                />
                <span class="absolute -bottom-1 -right-1 inline-flex items-center justify-center w-7 h-7 rounded-full bg-spot-coral text-white text-[10px] font-bold shadow-sm">
                  AI
                </span>
              </div>
              <div>
                <p class="text-[10px] uppercase tracking-[0.2em] text-spot-coral font-bold">SpotSeoul AI 가이드</p>
                <h3 class="text-base sm:text-lg font-black text-spot-navy">오늘은 어떤 서울 여행을 도와드릴까요?</h3>
                <p class="text-[11px] text-slate-500 mt-1 max-w-sm">
                  추천 코스, 명소 정보, 축제 일정까지 빠르게 안내해 드려요.
                </p>
              </div>
            </div>
            <div class="flex flex-wrap gap-2">
              <span class="inline-flex items-center gap-2 rounded-full bg-emerald-50 text-emerald-700 text-[10px] font-bold px-3 py-1 border border-emerald-100">
                <i class="fas fa-check-circle"></i> 실시간 추천
              </span>
              <span class="inline-flex items-center gap-2 rounded-full bg-slate-100 text-slate-700 text-[10px] font-semibold px-3 py-1 border border-slate-200">
                <i class="fas fa-map-marked-alt"></i> 여행 집중형
              </span>
            </div>
          </div>

          <div class="mt-4 rounded-[28px] bg-slate-50 border border-slate-200 p-4 min-h-[320px] overflow-hidden">
            <div ref="chatFeed" class="max-h-[420px] overflow-y-auto space-y-4 pr-2">
              <div
                v-for="(msg, index) in chatMessages"
                :key="index"
                :class="[
                  'flex gap-3 max-w-[85%] transition-all duration-300',
                  msg.role === 'user' ? 'ml-auto flex-row-reverse' : '',
                ]"
              >
                <div
                  v-if="msg.role === 'assistant'"
                  class="w-8 h-8 rounded-full bg-spot-navy flex items-center justify-center flex-shrink-0 overflow-hidden"
                >
                  <img src="/images/spoti2.png" alt="챗봇" class="w-full h-full object-cover" />
                </div>

                <div class="space-y-1">
                  <div
                    :class="[
                      'p-3.5 rounded-3xl leading-relaxed shadow-sm border text-xs',
                      msg.role === 'user'
                        ? 'bg-spot-navy text-white border-spot-navy'
                        : 'bg-white text-spot-navy border-spot-border',
                    ]"
                  >
                    <span class="whitespace-pre-wrap">{{ sanitizeEmojiText(msg.text) }}</span>

                    <!-- Recommended Spot Card -->
                    <div v-if="msg.spots && msg.spots.length > 0" class="mt-3.5 space-y-2">
                      <div
                        v-for="sp in msg.spots"
                        :key="sp.name"
                        @click="openSpotDetail(sp)"
                        class="p-3 bg-spot-bg rounded-3xl border border-spot-border hover:border-spot-coral/40 flex items-center justify-between gap-3 cursor-pointer transition-all"
                      >
                        <div class="min-w-0">
                          <h5 class="font-extrabold text-[11px] text-spot-navy truncate">
                            {{ sp.name }}
                          </h5>
                          <p class="text-[9px] text-slate-500 truncate">{{ sp.address }}</p>
                        </div>
                        <span
                          class="text-[9px] bg-spot-navy text-white font-extrabold px-2 py-0.5 rounded-full whitespace-nowrap"
                        >장소보기</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div v-if="chatLoading" class="flex gap-3 max-w-[85%]">
                <div
                  class="w-8 h-8 rounded-full bg-slate-300 flex items-center justify-center flex-shrink-0 overflow-hidden"
                >
                  <img src="/images/spoti2.png" alt="챗봇 로딩" class="w-full h-full object-cover" />
                </div>
                <div
                  class="p-3 bg-white text-slate-400 border border-spot-border rounded-2xl text-xs flex items-center gap-1.5"
                >
                  <i class="fas fa-spinner animate-spin"></i> 로컬 가이드 분석중...
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Predefined Quick preset buttons -->
        <div
          class="py-2.5 border-t border-spot-border flex flex-wrap gap-1.5 flex-shrink-0 bg-spot-bg/40 p-2.5 rounded-xl mb-2"
        >
          <button
            v-for="prompt in quickPrompts"
            :key="prompt"
            @click="usePresetPrompt(prompt)"
            class="bg-white hover:bg-spot-border/50 text-spot-navy border border-spot-border px-3 py-1.5 rounded-lg text-[10px] font-bold transition-all shadow-xs"
          >
            {{ prompt }}
          </button>
        </div>

        <!-- Input area -->
        <form
          @submit.prevent="sendChatMessage"
          class="p-2 border border-spot-border bg-white rounded-xl flex gap-2 flex-shrink-0 shadow-sm"
        >
          <input
            type="text"
            v-model="chatInput"
            placeholder="서울의 어떤 정보가 궁금하신가요? 명소명, 맛집, 축제를 검색해 보세요..."
            required
            class="flex-grow px-3 py-2 border-0 bg-transparent text-xs focus:outline-none text-spot-navy"
          />
          <button
            type="submit"
            class="w-9 h-9 bg-spot-navy hover:bg-slate-800 text-white rounded-lg flex items-center justify-center transition-all"
          >
            <i class="fas fa-paper-plane text-xs"></i>
          </button>
        </form>
      </div>
    </main>

    <!-- Detailed Spot Info Modal dialog -->
    <div
      v-if="detailModal.visible && detailModal.spot"
      class="fixed inset-0 z-50 flex items-center justify-center p-4"
    >
      <div class="fixed inset-0 bg-slate-950/60 backdrop-blur-xs" @click="closeSpotDetail"></div>
      <div
        class="relative bg-white rounded-2xl shadow-2xl border border-spot-border max-w-lg w-full overflow-hidden z-10"
      >
        <div
          class="h-48 bg-cover bg-center relative"
          :style="`background-image: url('${detailModal.spot.img}');`"
        >
          <div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"></div>
          <button
            @click="closeSpotDetail"
            class="absolute top-3 right-3 w-7 h-7 bg-black/45 hover:bg-black/75 text-white rounded-full flex items-center justify-center text-xs transition-colors"
          >
            <i class="fas fa-times"></i>
          </button>
          <div class="absolute bottom-3 left-3 text-white">
            <span
              class="text-[8px] bg-spot-coral text-white font-extrabold px-1.5 py-0.5 rounded tracking-widest uppercase"
            >
              {{ detailModal.spot.category }}
            </span>
            <h3 class="font-extrabold text-base sm:text-lg mt-1">{{ detailModal.spot.name }}</h3>
          </div>
        </div>
        <div class="p-5 space-y-4 text-xs">
          <p class="text-slate-600 leading-relaxed">{{ detailModal.spot.desc }}</p>
          <div class="bg-spot-bg p-3 rounded-xl border border-spot-border space-y-1.5">
            <p class="text-[10px] text-slate-500">
              <i class="fas fa-map-marker-alt text-spot-coral mr-1.5"></i
              ><strong>지번 주소:</strong> {{ detailModal.spot.address }}
            </p>
            <p class="text-[10px] text-slate-500">
              <i class="far fa-clock text-spot-coral mr-1.5"></i><strong>운영 시간:</strong> 09:00 -
              18:00 (시즌별 유동적)
            </p>
          </div>
          <div class="flex gap-2">
            <button
              @click="focusOnMapAndGo(detailModal.spot)"
              class="flex-grow py-2.5 bg-spot-navy text-white font-bold rounded-lg text-center shadow hover:bg-slate-800 transition-colors"
            >
              <i class="fas fa-compass mr-1"></i> 지도로 위치 보기
            </button>
            <button
              @click="askChatbotAbout(detailModal.spot)"
              class="py-2.5 px-4 bg-spot-coral text-white font-bold rounded-lg hover:bg-spot-coral/90 transition-colors inline-flex items-center gap-2"
            >
              <img src="/images/spoti2.png" alt="챗봇" class="w-4 h-4 rounded-full object-cover" />
              AI 챗봇 문의
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Password Modal for Anonymous Board Management -->
    <div
      v-if="passwordModal.visible"
      class="fixed inset-0 z-50 flex items-center justify-center p-4"
    >
      <div class="fixed inset-0 bg-slate-950/65" @click="passwordModal.visible = false"></div>
      <div
        class="relative bg-white rounded-2xl shadow-2xl border border-spot-border max-w-xs w-full p-5 space-y-4 z-10 text-center"
      >
        <div
          class="w-10 h-10 rounded-full bg-rose-50 text-rose-500 flex items-center justify-center mx-auto text-sm"
        >
          <i class="fas fa-lock"></i>
        </div>
        <div>
          <h4 class="font-bold text-sm text-spot-navy">비밀번호 확인</h4>
          <p class="text-[10px] text-slate-400 mt-1">
            글을 등록했을 때 지정한 비밀번호 4자리를 기입해주세요.
          </p>
        </div>
        <input
          type="password"
          v-model="passwordModal.input"
          placeholder="4자리 숫자"
          maxlength="4"
          class="w-full text-center tracking-widest text-lg font-mono px-3 py-2 border border-spot-border bg-spot-bg rounded-lg focus:outline-none text-spot-navy"
        />
        <div class="grid grid-cols-2 gap-2 text-xs">
          <button
            @click="passwordModal.visible = false"
            class="py-2 bg-slate-100 text-slate-600 rounded-lg font-bold"
          >
            취소
          </button>
          <button
            @click="verifyPassword"
            class="py-2 bg-spot-navy text-white rounded-lg font-bold hover:bg-slate-800"
          >
            확인
          </button>
        </div>
      </div>
    </div>

    <div
      v-if="commentPasswordModal.visible"
      class="fixed inset-0 z-[60] flex items-center justify-center p-4"
    >
      <div class="fixed inset-0 bg-slate-950/65" @click="commentPasswordModal.visible = false"></div>
      <div class="relative bg-white rounded-2xl shadow-2xl border border-spot-border max-w-xs w-full p-5 space-y-4 z-10 text-center">
        <div class="w-10 h-10 rounded-full bg-amber-50 text-amber-500 flex items-center justify-center mx-auto text-sm">
          <i class="fas fa-lock"></i>
        </div>
        <div>
          <h4 class="font-bold text-sm text-spot-navy">댓글 비밀번호 확인</h4>
          <p class="text-[10px] text-slate-400 mt-1">등록 시 입력한 비밀번호 4자리를 입력해주세요.</p>
        </div>
        <input
          type="password"
          v-model="commentPasswordModal.input"
          placeholder="4자리 숫자"
          maxlength="4"
          class="w-full text-center tracking-widest text-lg font-mono px-3 py-2 border border-spot-border bg-spot-bg rounded-lg focus:outline-none text-spot-navy"
        />
        <div class="grid grid-cols-2 gap-2 text-xs">
          <button @click="commentPasswordModal.visible = false" class="py-2 bg-slate-100 text-slate-600 rounded-lg font-bold">취소</button>
          <button @click="verifyCommentPassword" class="py-2 bg-spot-navy text-white rounded-lg font-bold hover:bg-slate-800">확인</button>
        </div>
      </div>
    </div>

    <div v-if="commentEditModal.visible" class="fixed inset-0 z-[70] flex items-center justify-center p-4">
      <div class="fixed inset-0 bg-slate-950/65" @click="commentEditModal.visible = false"></div>
      <div class="relative bg-white rounded-2xl shadow-2xl border border-spot-border max-w-md w-full p-5 space-y-4 z-10">
        <div class="flex items-center justify-between">
          <h4 class="font-bold text-sm text-spot-navy">댓글 수정</h4>
          <button @click="commentEditModal.visible = false" class="text-xs text-slate-400">닫기</button>
        </div>
        <textarea
          v-model="commentEditModal.content"
          rows="4"
          placeholder="수정할 댓글 내용을 입력하세요"
          class="w-full px-3 py-2 border border-spot-border bg-spot-bg rounded-lg text-xs focus:outline-none text-spot-navy"
        ></textarea>
        <div class="flex gap-2 justify-end">
          <button @click="commentEditModal.visible = false" class="px-3 py-2 bg-slate-100 text-slate-600 rounded-lg text-xs font-bold">취소</button>
          <button @click="submitCommentEdit" class="px-3 py-2 bg-spot-navy text-white rounded-lg text-xs font-bold hover:bg-slate-800">저장</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineOptions({ name: 'SpotSeoulApp' })

import { nextTick, onBeforeUnmount, onMounted, reactive, ref } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

const toast = reactive({ visible: false, message: '' })
const showToast = (msg) => {
  toast.message = msg
  toast.visible = true
  setTimeout(() => {
    toast.visible = false
  }, 3000)
}

const sanitizeEmojiText = (text) => {
  if (!text || typeof text !== 'string') return ''

  return text
    .replace(/([\u{1F300}-\u{1FAFF}\u{2600}-\u{27BF}])/gu, ' ')
    .replace(/\s+/g, ' ')
    .trim()
}

// FastAPI Server Base URL - 환경변수 또는 배포 주소를 입력하세요
const apiBaseUrl = ref(import.meta.env.VITE_API_BASE_URL || 'https://spotseoul-backend.onrender.com')
const customApiKey = ref('')
const mobileMenuOpen = ref(false)

const activeTab = ref('home')
const tabItems = [
  { id: 'home', name: '홈' },
  { id: 'spots', name: '명소' },
  { id: 'map', name: '지도' },
  { id: 'courses', name: '코스' },
  { id: 'board', name: '게시판' },
  { id: 'chatbot', name: '챗봇' },
]

const loading = ref(false)
const selectedCategory = ref('전체')
const spotSearchQuery = ref('')

// Home categories design-sync buttons
const homeCategories = [
  {
    name: '관광지',
    icon: 'fas fa-landmark',
    color: 'bg-blue-600',
    desc: '역사 · 문화 · 자연 명소',
    image: '/images/img_tourist_spot.jpg',
  },
  {
    name: '문화시설',
    icon: 'fas fa-palette',
    color: 'bg-purple-600',
    desc: '박물관 · 미술관 · 공연장',
    image: '/images/img_culture.jpg',
  },
  {
    name: '레포츠',
    icon: 'fas fa-route',
    color: 'bg-green-600',
    desc: '한강 · 공원 · 등산 코스',
    image: '/images/img_leisure_sports.jpg',
  },
  {
    name: '쇼핑',
    icon: 'fas fa-heart',
    color: 'bg-red-600',
    desc: '명동 · 홍대 · 코엑스몰',
    image: '/images/img_shopping.jpg',
  },
  {
    name: '축제',
    icon: 'fas fa-calendar',
    color: 'bg-orange-500',
    desc: '계절별 특별 행사',
    image: '/images/img_festival.jpg',
  },
  {
    name: '숙박',
    icon: 'fas fa-location-dot',
    color: 'bg-cyan-600',
    desc: '호텔 · 게스트하우스',
    image: '/images/img_accomodation.jpg',
  },
]
// Reactive lists driven by FastAPI backend data
const places = ref([])
const courseGroups = ref([])
const posts = ref([])

const courseSearchQuery = ref('')
const selectedCourseTheme = ref('all')
const selectedCourseDuration = ref('all')
const courseSortOrder = ref('latest')
const courseDetail = ref(null)
const selectedBoardCategory = ref('전체')
const boardSearchQuery = ref('')

const courseThemeOptions = [
  { value: 'all', label: '전체' },
  { value: 'history', label: '역사' },
  { value: 'date', label: '데이트' },
  { value: 'healing', label: '힐링' },
  { value: 'night', label: '야경' },
  { value: 'hangang', label: '한강' },
]

const courseDurationOptions = [
  { value: 'all', label: '전체' },
  { value: 'day', label: '당일치기' },
  { value: 'one_night', label: '1박 2일' },
  { value: 'long_trip', label: '2박 3일+' },
]

// ==================== Backend API 연동 비동기 메소드 ====================


const fetchInitialData = async () => {
  try {
    // 홈 화면 및 기타 화면 데이터 일괄 호출
    const resSpots = await fetch(`${apiBaseUrl.value}/api/spots`)
    if (resSpots.ok) places.value = await resSpots.json()

    const resCourses = await fetch(`${apiBaseUrl.value}/api/travel-courses`)
    if (resCourses.ok) courseGroups.value = await resCourses.json()

    await fetchPosts()
  } catch (e) {
    console.error('API 연동 실패로 로컬 메모리 상태에 진입합니다.', e)
  }
}

const selectSpotCategory = async (cat) => {
  selectedCategory.value = cat
  await searchSpots()
}

const searchSpots = async () => {
  loading.value = true
  try {
    const url = `${apiBaseUrl.value}/api/spots?category=${encodeURIComponent(selectedCategory.value)}&query=${encodeURIComponent(spotSearchQuery.value)}`
    const res = await fetch(url)
    if (res.ok) places.value = await res.json()
  } catch (e) {
    console.error(e)
  } finally {
    loading.value = false
  }
}

const selectCourseTheme = async (theme) => {
  selectedCourseTheme.value = theme
  await loadTravelCourses()
}

const selectCourseDuration = async (duration) => {
  selectedCourseDuration.value = duration
  await loadTravelCourses()
}

const loadTravelCourses = async () => {
  try {
    const params = new URLSearchParams()
    if (courseSearchQuery.value) params.set('search', courseSearchQuery.value)
    if (selectedCourseTheme.value !== 'all') params.set('theme', selectedCourseTheme.value)
    if (selectedCourseDuration.value !== 'all') params.set('duration', selectedCourseDuration.value)
    if (courseSortOrder.value) params.set('order', courseSortOrder.value)

    const url = `${apiBaseUrl.value}/api/travel-courses${params.toString() ? `?${params.toString()}` : ''}`
    const res = await fetch(url)
    if (res.ok) courseGroups.value = await res.json()
  } catch (e) {
    console.error('Travel course load failed', e)
  }
}

const selectBoardCategory = async (cat) => {
  selectedBoardCategory.value = cat
  await fetchPosts()
}

const fetchPosts = async () => {
  try {
    const url = `${apiBaseUrl.value}/api/posts?category=${encodeURIComponent(selectedBoardCategory.value)}&query=${encodeURIComponent(boardSearchQuery.value)}`
    const res = await fetch(url)
    if (res.ok) {
      const fetchedPosts = await res.json()
      posts.value = [...fetchedPosts]
        .map((post) => ({ ...post, views: Number(post.views || 0) }))
        .sort((a, b) => {
          const aTime = new Date(a.created_at || 0).getTime()
          const bTime = new Date(b.created_at || 0).getTime()
          return bTime - aTime || b.id - a.id
        })
    }
  } catch (e) {
    console.error(e)
  }
}

// ==================== Forum Board CRUD Handlers ====================

const boardSubView = ref('list')
const currentPost = ref(null)

const writeForm = reactive({ category: '관광지', title: '', content: '', author: '', password: '' })
const commentForm = reactive({ author: '', password: '', content: '' })
const commentEditId = ref(null)
const passwordModal = reactive({ visible: false, action: 'edit', postId: null, input: '' })
const commentPasswordModal = reactive({ visible: false, action: 'edit', commentId: null, input: '' })
const commentEditModal = reactive({ visible: false, commentId: null, content: '' })

const goToPostDetail = async (post) => {
  try {
    const res = await fetch(`${apiBaseUrl.value}/api/posts/${post.id}`)
    if (res.ok) {
      currentPost.value = await res.json()
      boardSubView.value = 'detail'
      await fetchPosts()
      pushHistoryState()
    }
  } catch (e) {
    console.error(e)
  }
}

const openCourseDetail = (course) => {
  courseDetail.value = course
}

const closeCourseDetail = () => {
  courseDetail.value = null
}

const backToBoardList = () => {
  boardSubView.value = 'list'
  pushHistoryState()
}

const openBoardWrite = () => {
  boardSubView.value = 'write'
  pushHistoryState()
}

const cancelWritePost = () => {
  boardSubView.value = 'list'
  pushHistoryState()
}

const createPost = async () => {
  if (writeForm.password.length !== 4) {
    showToast('비밀번호는 반드시 숫자 4자리여야 합니다.')
    return
  }

  const isEditing = boardSubView.value === 'write' && currentPost.value && currentPost.value.id
  const url = isEditing
    ? `${apiBaseUrl.value}/api/posts/${currentPost.value.id}`
    : `${apiBaseUrl.value}/api/posts`
  const method = isEditing ? 'PUT' : 'POST'

  try {
    const res = await fetch(url, {
      method,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(writeForm),
    })
    if (res.ok) {
      showToast(isEditing ? '기존 글이 성공적으로 수정되었습니다.' : '익명 정보 공유글이 실시간 업로드되었습니다!')
      writeForm.category = '관광지'
      writeForm.title = ''
      writeForm.content = ''
      writeForm.author = ''
      writeForm.password = ''
      boardSubView.value = 'list'
      currentPost.value = null
      pushHistoryState()
      await fetchPosts()
    } else {
      const errorData = await res.json().catch(() => ({}))
      showToast(errorData.detail || (isEditing ? '글 수정 서버 통신 오류' : '글 작성 서버 통신 오류'))
    }
  } catch (e) {
    showToast(isEditing ? '글 수정 서버 통신 오류' : '글 작성 서버 통신 오류')
  }
}

const openPasswordModal = (action) => {
  passwordModal.action = action
  passwordModal.postId = currentPost.value.id
  passwordModal.input = ''
  passwordModal.visible = true
}

const verifyPassword = async () => {
  try {
    const resVerify = await fetch(`${apiBaseUrl.value}/api/posts/${passwordModal.postId}/verify`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ password: passwordModal.input }),
    })

    if (!resVerify.ok) {
      showToast('비밀번호가 불일치합니다. 다시 기입해 주세요.')
      return
    }

    passwordModal.visible = false

    if (passwordModal.action === 'edit') {
      // 수정 모드 진입
      writeForm.category = currentPost.value.category
      writeForm.title = currentPost.value.title
      writeForm.content = currentPost.value.content
      writeForm.author = currentPost.value.author
      writeForm.password = passwordModal.input

      // 기존 데이터 임시 백업 및 프런트 수정 뷰로 전환 후 API 갱신 처리
      boardSubView.value = 'write'
      pushHistoryState()
      showToast('수정 권한 확인 완료. 정보를 변경해주세요.')
    } else if (passwordModal.action === 'delete') {
      // 삭제 실행
      const resDel = await fetch(`${apiBaseUrl.value}/api/posts/${passwordModal.postId}`, {
        method: 'DELETE',
        headers: { password: passwordModal.input },
      })
      if (resDel.ok) {
        showToast('해당 익명 공유글이 완벽히 삭제되었습니다.')
        boardSubView.value = 'list'
        pushHistoryState()
        await fetchPosts()
      }
    }
  } catch (e) {
    showToast('서버 검증 중 통신 에러가 발생했습니다.')
  }
}

const resetCommentForm = () => {
  commentForm.author = ''
  commentForm.content = ''
  commentForm.password = ''
  commentEditId.value = null
}

const openCommentPasswordModal = (action, commentId) => {
  commentPasswordModal.action = action
  commentPasswordModal.commentId = commentId
  commentPasswordModal.input = ''
  commentPasswordModal.visible = true
}

const verifyCommentPassword = async () => {
  if (commentPasswordModal.input.length !== 4) {
    showToast('비밀번호는 4자리 숫자여야 합니다.')
    return
  }

  try {
    const res = await fetch(`${apiBaseUrl.value}/api/posts/${currentPost.value.id}/comments/${commentPasswordModal.commentId}/verify`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ password: commentPasswordModal.input }),
    })

    if (!res.ok) {
      showToast('비밀번호 검증 중 오류가 발생했습니다.')
      return
    }

    const data = await res.json()
    if (!data.verified) {
      showToast('비밀번호가 일치하지 않습니다.')
      return
    }

    commentPasswordModal.visible = false

    if (commentPasswordModal.action === 'delete') {
      await deleteComment(commentPasswordModal.commentId, commentPasswordModal.input)
    } else {
      const targetComment = currentPost.value?.comments?.find((item) => item.id === commentPasswordModal.commentId)
      if (!targetComment) return
      commentEditModal.commentId = targetComment.id
      commentEditModal.content = targetComment.content
      commentEditModal.visible = true
    }
  } catch (e) {
    showToast('비밀번호 검증 중 오류가 발생했습니다.')
  }
}

const submitComment = async () => {
  if (commentForm.password.length !== 4) {
    showToast('댓글 권한번호 4자리를 정확히 입력해주세요.')
    return
  }

  const isEditing = Boolean(commentEditId.value)
  const url = isEditing
    ? `${apiBaseUrl.value}/api/posts/${currentPost.value.id}/comments/${commentEditId.value}`
    : `${apiBaseUrl.value}/api/posts/${currentPost.value.id}/comments`
  const method = isEditing ? 'PUT' : 'POST'

  try {
    const res = await fetch(url, {
      method,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(commentForm),
    })
    if (res.ok) {
      showToast(isEditing ? '댓글이 성공적으로 수정되었습니다.' : '익명 피드백 댓글이 실시간 등록되었습니다!')
      resetCommentForm()
      await goToPostDetail(currentPost.value)
    } else {
      const errorData = await res.json().catch(() => ({}))
      showToast(errorData.detail || (isEditing ? '댓글 수정 서버 통신 오류' : '댓글 업로드 통신 에러'))
    }
  } catch (e) {
    showToast(isEditing ? '댓글 수정 서버 통신 오류' : '댓글 업로드 통신 에러')
  }
}

const submitCommentEdit = async () => {
  if (!commentEditModal.content.trim()) {
    showToast('수정할 댓글 내용을 입력해주세요.')
    return
  }

  const targetComment = currentPost.value?.comments?.find((item) => item.id === commentEditModal.commentId)
  if (!targetComment) return

  try {
    const res = await fetch(`${apiBaseUrl.value}/api/posts/${currentPost.value.id}/comments/${commentEditModal.commentId}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        author: targetComment.author,
        password: commentPasswordModal.input,
        content: commentEditModal.content.trim(),
      }),
    })

    if (res.ok) {
      showToast('댓글이 수정되었습니다.')
      commentEditModal.visible = false
      commentEditModal.commentId = null
      commentEditModal.content = ''
      commentPasswordModal.input = ''
      await goToPostDetail(currentPost.value)
    } else {
      const errorData = await res.json().catch(() => ({}))
      showToast(errorData.detail || '댓글 수정 서버 통신 오류')
    }
  } catch (e) {
    showToast('댓글 수정 서버 통신 오류')
  }
}

const deleteComment = async (commentId, password = '') => {
  if (!password) {
    openCommentPasswordModal('delete', commentId)
    return
  }

  try {
    const res = await fetch(`${apiBaseUrl.value}/api/posts/${currentPost.value.id}/comments/${commentId}`, {
      method: 'DELETE',
      headers: { password },
    })
    if (res.ok) {
      showToast('댓글이 삭제되었습니다.')
      commentPasswordModal.visible = false
      commentPasswordModal.input = ''
      await goToPostDetail(currentPost.value)
    } else {
      const errorData = await res.json().catch(() => ({}))
      showToast(errorData.detail || '댓글 삭제 서버 통신 오류')
    }
  } catch (e) {
    showToast('댓글 삭제 서버 통신 오류')
  }
}

// ==================== Interactive Leaflet.js Mapping Handlers ====================

// Map variables
let mapInstance = null
let markerGroup = null
const mapSearchQuery = ref('')
const mapFilterCategory = ref('전체')
const selectedMapSpotId = ref(null)
const mapPlaces = ref([])

const selectMapCategory = (cat) => {
  mapFilterCategory.value = cat
  filterMapPlaces()
}

const filterMapPlaces = () => {
  mapPlaces.value = places.value.filter((p) => {
    const matchesCategory =
      mapFilterCategory.value === '전체' || p.category === mapFilterCategory.value
    const q = mapSearchQuery.value.trim().toLowerCase()
    const matchesQuery = !q || p.name.toLowerCase().includes(q)
    return matchesCategory && matchesQuery
  })
  renderMapMarkers()
}

const initLeafletMap = () => {
  if (mapInstance) {
    mapInstance.remove()
    mapInstance = null
  }

  // Center on Seoul
  mapInstance = L.map('map-container').setView([37.5512, 126.9882], 12)
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors',
  }).addTo(mapInstance)

  markerGroup = L.layerGroup().addTo(mapInstance)
  filterMapPlaces()
}

const renderMapMarkers = () => {
  if (!markerGroup) return
  markerGroup.clearLayers()

  const customIcon = L.icon({
    iconUrl: '/images/marker-pin.svg',
    iconSize: [32, 40],
    iconAnchor: [16, 40],
    popupAnchor: [0, -34],
  })

  mapPlaces.value.forEach((spot) => {
    const marker = L.marker([spot.lat, spot.lng], { icon: customIcon }).addTo(markerGroup)
    marker.bindPopup(`
                        <div class="p-1 space-y-1 text-xs">
                            <strong class="text-spot-navy font-bold block">${spot.name}</strong>
                            <span class="text-[9px] bg-spot-coral text-white px-1.5 py-0.2 rounded">${spot.category}</span>
                            <p class="text-[9px] text-slate-500 mt-1">${spot.address}</p>
                        </div>
                    `)
    marker.on('click', () => {
      selectedMapSpotId.value = spot.id
    })
  })
}

const focusMapOn = (spot) => {
  selectedMapSpotId.value = spot.id
  if (mapInstance) {
    mapInstance.setView([spot.lat, spot.lng], 14)
    // Find marker to open popup
    markerGroup.eachLayer((layer) => {
      if (layer.getLatLng().lat === spot.lat && layer.getLatLng().lng === spot.lng) {
        layer.openPopup()
      }
    })
  }
}

const focusOnMapAndGo = (spot) => {
  activeTab.value = 'map'
  nextTick(() => {
    initLeafletMap()
    focusMapOn(spot)
  })
  closeSpotDetail()
}

const searchPlaceAndFocus = (name) => {
  const target = places.value.find((p) => p.name.includes(name) || name.includes(p.name))
  if (target) {
    focusOnMapAndGo(target)
  } else {
    showToast(`"${name}"의 정밀 지리 좌표를 계산하고 있습니다.`)
  }
}

// ==================== Live Intelligent Chatbot Handlers ====================

// AI Chatbot Logic
const chatInput = ref('')
const chatFeed = ref(null)
const chatLoading = ref(false)
const chatMessages = ref([
  {
    role: 'assistant',
    text: sanitizeEmojiText('안녕하세요! 서울 여행의 실시간 동반자 SpotSeoul 챗봇입니다.\n'),
  },
])

const quickPrompts = [
  '경복궁 입장료 알려줘',
  '서울 맛집 추천해줘',
  '이번 달 축제 뭐 있어?',
  '지하철 이용법 알려줘',
]

const askChatbotAbout = (spot) => {
  switchTab('chatbot')
  chatInput.value = `"${spot.name}"에 대해서 자세한 정보 알려줘!`
  sendChatMessage()
}

const usePresetPrompt = (prompt) => {
  chatInput.value = prompt
  sendChatMessage()
}

const sendChatMessage = async () => {
  if (!chatInput.value.trim()) return
  const userQuery = chatInput.value
  chatMessages.value.push({ role: 'user', text: sanitizeEmojiText(userQuery) })
  chatInput.value = ''
  chatLoading.value = true

  nextTick(() => {
    if (chatFeed.value) chatFeed.value.scrollTop = chatFeed.value.scrollHeight
  })

  try {
    const res = await fetch(`${apiBaseUrl.value}/api/chat`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        query: userQuery,
        custom_api_key: customApiKey.value,
      }),
    })

    if (res.ok) {
      const data = await res.json()
      chatMessages.value.push({
        role: 'assistant',
        text: sanitizeEmojiText(data.text),
        spots: data.spots,
      })
    } else {
      chatMessages.value.push({
        role: 'assistant',
        text: sanitizeEmojiText('주의: 백엔드 서버는 연결되었으나 가이드 연동 과정에서 에러가 발생했습니다.'),
      })
    }
  } catch (e) {
    chatMessages.value.push({
      role: 'assistant',
      text: sanitizeEmojiText('주의: 현재 입력된 주소로 FastAPI 서버에 접근할 수 없습니다. 상단 [서버 & AI 설정]에서 포트가 일치하는지 점검해 주세요!'),
    })
  } finally {
    chatLoading.value = false
    nextTick(() => {
      if (chatFeed.value) chatFeed.value.scrollTop = chatFeed.value.scrollHeight
    })
  }
}

// Details modal controls
const detailModal = reactive({ visible: false, spot: null })
const openSpotDetail = (spot) => {
  detailModal.spot = spot
  detailModal.visible = true
}
const closeSpotDetail = () => {
  detailModal.visible = false
  detailModal.spot = null
}

// Navigation tab switcher
const switchTab = (tabId) => {
  activeTab.value = tabId
  mobileMenuOpen.value = false
  if (tabId === 'spots') {
    searchSpots()
  }
  if (tabId === 'map') {
    nextTick(() => {
      initLeafletMap()
    })
  }
  pushHistoryState()
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

const goToSpotsWithCategory = (cat) => {
  selectedCategory.value = cat
  switchTab('spots')
}

const getAppState = () => ({
  activeTab: activeTab.value,
  mobileMenuOpen: false,
  boardSubView: boardSubView.value,
  currentPostId: currentPost.value?.id ?? null,
  selectedBoardCategory: selectedBoardCategory.value,
  boardSearchQuery: boardSearchQuery.value,
  selectedCategory: selectedCategory.value,
})

const replaceHistoryState = () => {
  const state = getAppState()
  window.history.replaceState(state, '', window.location.href)
}

const pushHistoryState = () => {
  const state = getAppState()
  window.history.pushState(state, '', window.location.href)
}

const restoreHistoryState = async (state) => {
  if (!state) {
    activeTab.value = 'home'
    boardSubView.value = 'list'
    return
  }

  activeTab.value = state.activeTab || 'home'
  selectedBoardCategory.value = state.selectedBoardCategory || '전체'
  boardSearchQuery.value = state.boardSearchQuery || ''
  selectedCategory.value = state.selectedCategory || '전체'
  mobileMenuOpen.value = false

  if (activeTab.value === 'board') {
    boardSubView.value = state.boardSubView || 'list'
    if (boardSubView.value === 'detail' && state.currentPostId) {
      await loadPostById(state.currentPostId)
    }
  }

  if (activeTab.value === 'spots') {
    await searchSpots()
  }

  if (activeTab.value === 'map') {
    nextTick(() => {
      initLeafletMap()
    })
  }
}

const handlePopState = async (event) => {
  await restoreHistoryState(event.state)
}

const loadPostById = async (postId) => {
  try {
    const res = await fetch(`${apiBaseUrl.value}/api/posts/${postId}`)
    if (res.ok) {
      currentPost.value = await res.json()
      boardSubView.value = 'detail'
    }
  } catch (e) {
    console.error('게시글 복원 실패', e)
  }
}

// Helpers
const getBadgeClass = (cat) => {
  const maps = {
    관광지: 'bg-indigo-50 text-indigo-700 border border-indigo-200',
    문화시설: 'bg-emerald-50 text-emerald-700 border border-emerald-200',
    레포츠: 'bg-teal-50 text-teal-700 border border-teal-200',
    쇼핑: 'bg-amber-50 text-amber-700 border border-amber-200',
    축제: 'bg-pink-50 text-pink-700 border border-pink-200',
    맛집: 'bg-rose-50 text-rose-700 border border-rose-200',
    숙박: 'bg-blue-50 text-blue-700 border border-blue-200',
    기타: 'bg-slate-100 text-slate-700 border border-slate-200',
  }
  return maps[cat] || 'bg-slate-100 text-slate-700'
}

const timeAgo = (isoStr) => {
  if (!isoStr) return '방금 전'

  const normalized = String(isoStr).trim().replace(' ', 'T')
  const hasTimeZone = /[zZ]|[+-]\d{2}:\d{2}$/.test(normalized)
  const parsed = new Date(hasTimeZone ? normalized : `${normalized}Z`)

  if (Number.isNaN(parsed.getTime())) return '방금 전'

  const diff = Date.now() - parsed.getTime()
  const min = Math.floor(diff / 60000)
  if (min < 1) return '방금 전'
  if (min < 60) return `${min}분 전`
  const hr = Math.floor(min / 60)
  if (hr < 24) return `${hr}시간 전`
  return `${Math.floor(hr / 24)}일 전`
}

onMounted(() => {
  replaceHistoryState()``
  window.addEventListener('popstate', handlePopState)
  fetchInitialData()
})

onBeforeUnmount(() => {
  window.removeEventListener('popstate', handlePopState)
  if (mapInstance) {
    mapInstance.remove()
    mapInstance = null
  }
})
</script>

<style>
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

::-webkit-scrollbar-track {
  background: #f5f2eb;
}

::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 999px;
}

::-webkit-scrollbar-thumb:hover {
  background: #94a3b8;
}
</style>
