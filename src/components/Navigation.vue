<template>
    <nav
        class="uk-navbar-container uk-container-expand uk-position-relative"
        :style="[{ background: backgroundColor, colour: foregroundColor }]"
        uk-navbar
    >
        <div class="uk-navbar-left">
            <router-link class="uk-navbar-item uk-logo uk-text-bold" :style="[{ colour: foregroundColor }]" to="/"
                ><img
                    alt="logo"
                    src="/img/icons/logo.svg"
                    height="32"
                    width="32"
                    style="margin-bottom: 6px; margin-right: -13px"
                />iped</router-link
            >
        </div>
        <div class="uk-navbar-center uk-flex uk-visible@m">
            <input
                class="uk-input uk-width-medium"
                type="text"
                role="search"
                :title="$t('actions.search')"
                :placeholder="$t('actions.search')"
                v-model="searchText"
                @keyup="onKeyUp"
                @focus="onInputFocus"
                @blur="onInputBlur"
            />
        </div>
        <div class="uk-navbar-right">
            <ul class="uk-navbar-nav">
                <li>
                    <router-link to="/preferences" v-t="'titles.preferences'" />
                </li>
                <li v-if="shouldShowLogin">
                    <router-link to="/login" v-t="'titles.login'" />
                </li>
                <li v-if="shouldShowLogin">
                    <router-link to="/register" v-t="'titles.register'" />
                </li>
                <li v-if="shouldShowHistory">
                    <router-link to="/history" v-t="'titles.history'" />
                </li>
                <li v-if="authenticated">
                    <router-link to="/feed" v-t="'titles.feed'" />
                </li>
            </ul>
        </div>
    </nav>
    <div class="uk-container-expand uk-hidden@m">
        <input
            class="uk-input"
            type="text"
            role="search"
            :title="$t('actions.search')"
            :placeholder="$t('actions.search')"
            v-model="searchText"
            @keyup="onKeyUp"
            @focus="onInputFocus"
            @blur="onInputBlur"
        />
    </div>
    <SearchSuggestions
        v-show="searchText && suggestionsVisible"
        :searchText="searchText"
        @searchchange="onSearchTextChange"
        ref="searchSuggestions"
    />
</template>

<script>
import SearchSuggestions from "@/components/SearchSuggestions";

export default {
    components: {
        SearchSuggestions,
    },
    data() {
        return {
            searchText: "",
            suggestionsVisible: false,
        };
    },
    computed: {
        shouldShowLogin(_this) {
            return _this.getAuthToken() == null;
        },
        shouldShowHistory(_this) {
            return _this.getPreferenceBoolean("watchHistory", false);
        },
    },
    methods: {
        onKeyUp(e) {
            if (e.key === "Enter") {
                e.target.blur();
                this.$router.push({
                    name: "SearchResults",
                    query: { search_query: this.searchText },
                });
                return;
            } else if (e.key === "ArrowUp" || e.key === "ArrowDown") {
                e.preventDefault();
            }
            this.$refs.searchSuggestions.onKeyUp(e);
        },
        onInputFocus() {
            this.suggestionsVisible = true;
        },
        onInputBlur() {
            this.suggestionsVisible = false;
        },
        onSearchTextChange(searchText) {
            this.searchText = searchText;
        },
    },
};
</script>

<style></style>
