# frozen_string_literal: true

source "https://rubygems.org"

gem "jekyll"

gem "course-ssg", git: "https://codeberg.org/pcantrell/course-ssg", branch: "main"

gem "superfluous", git: "https://codeberg.org/pcantrell/superfluous.git", branch: "main"

gem "haml", "~> 6.2"
gem "sass-embedded", "~> 1.83"

gem "adsf", git: "https://github.com/pcantrell/adsf", branch: "superfluous"
gem "adsf-live", git: "https://github.com/pcantrell/adsf", branch: "superfluous"

# For parsing and transforming src/data
gem "nokogiri", "~> 1.18"
gem "nokogiri_traversal", git: "https://codeberg.org/pcantrell/nokogiri_traversal.git", branch: "main"
gem "fastimage", "~> 2.4"
gem "kramdown", "~> 2.5"
gem "kramdown-parser-gfm", "~> 1.1"

# For syntax highlighting
gem "rouge", "~> 4.5"

# For events calendar
gem "icalendar", "~> 2.11"
# main branch has a bug fix not yet released as of 1.2.0 for an issue where
# recurring event end times are only a fraction of a second after the start time:
gem "icalendar-recurrence", git: "https://github.com/icalendar/icalendar-recurrence", branch: "main"
