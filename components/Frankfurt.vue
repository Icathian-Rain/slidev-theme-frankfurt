<template>
	<header class="absolute top-0 left-0 right-0" >
        <div class="flex shadow-black shadow-md text-sm px-4 -mx-4 z-2" style="background:black">
            <div v-for="(sec, i) in sections" class="flex-1 py-1 px-3"
			 :class="sec[1].includes($slidev.nav.currentPage - 1) ? 'text-white' : 'text-stone'" :key="i">
                <div>{{ sec[0] || "&emsp;" }}</div>
                <div>
                    <span v-for="p in sec[1]" :key="p">
                        <Link :to="p + 1" style="color:unset">{{ $slidev.nav.currentPage == p + 1 ? "●" : "○" }}</Link>
                    </span>
                </div>
            </div>
        </div>
		
        <h1> {{ pageSections[$slidev.nav.currentPage-2] }}</h1>
	</header>
</template>

<script setup lang="ts">
	import { computed } from "vue";

	import type { SlideInfoBase } from "@slidev/types";

	const sections = computed(() => {
		const result: [string, number[]][] = [];
		let pages: number[] = [];
		let title = "";
		for(let i = 1; i < $slidev.nav.slides.length; i++) {
			const slide = $slidev.nav.slides[i];
			const section = (slide.meta?.slide as SlideInfoBase)?.frontmatter?.section;
			if(section && section != title) {
				if(pages.length > 0) result.push([title, pages]);
				pages = [];
				title = section;
			}
			pages.push(i);
		}
		result.push([title, pages]);
		return result;
	});

    const pageSections = computed(()=> {
        const result: string[] = [];
        let title = "";
        for(let i = 1; i < $slidev.nav.slides.length; i++) {
			const slide = $slidev.nav.slides[i];
			const section = (slide.meta?.slide as SlideInfoBase)?.frontmatter?.section;
			if(section && section != title) {
				title = section;
			}
            result.push(title);
		}
        return result;
    })
</script>

<style scoped>
h1 {
    @apply text-white shadow-md;
    background: #3333B3;
    position: relative;
    font-size: xx-large;
    margin-left: -5px;
    padding: 0rem 0rem 5px 25px;
    width: 1000%;
    --un-shadow-opacity: 1;
    --un-shadow-color: #3333B3;
}
</style>