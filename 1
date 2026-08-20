#!/bin/bash

# ============================================================
# NORTHSTAR SERVER HEALTH CHECK
# ============================================================
# Purpose:
#   Monitor CPU, RAM, disk usage and hard-drive health.
#
# Exit codes:
#   0 = HEALTHY
#   1 = WARNING
#   2 = CRITICAL
# ============================================================

# -----------------------------
# Configuration / thresholds
# -----------------------------

CPU_WARNING=70
CPU_CRITICAL=90

RAM_WARNING=80
RAM_CRITICAL=90

DISK_WARNING=80
DISK_CRITICAL=90

# Overall status starts as HEALTHY.
OVERALL_STATUS=0


# -----------------------------
# Identify the server
# -----------------------------

SERVER_NAME=$(hostname)
DATE=$(date '+%Y-%m-%d %H:%M:%S')


# -----------------------------
# Collect CPU information
# -----------------------------

# /proc/stat contains cumulative CPU statistics.
# We take two samples one second apart and calculate
# the percentage of time the CPU was busy.

read -r cpu user nice system idle iowait irq softirq steal guest guest_nice < /proc/stat

TOTAL1=$((user + nice + system + idle + iowait + irq + softirq + steal))
IDLE1=$((idle + iowait))

sleep 1

read -r cpu user nice system idle iowait irq softirq steal guest guest_nice < /proc/stat

TOTAL2=$((user + nice + system + idle + iowait + irq + softirq + steal))
IDLE2=$((idle + iowait))

TOTAL_DIFF=$((TOTAL2 - TOTAL1))
IDLE_DIFF=$((IDLE2 - IDLE1))

if [ "$TOTAL_DIFF" -gt 0 ]; then
    CPU_USAGE=$((100 * (TOTAL_DIFF - IDLE_DIFF) / TOTAL_DIFF))
else
    CPU_USAGE=0
fi


# -----------------------------
# Collect RAM information
# -----------------------------

# free -m reports memory in MB.
# We calculate RAM usage from total and available memory.

read -r TOTAL_RAM AVAILABLE_RAM <<< \
    "$(free -m | awk '/^Mem:/ {print $2, $7}')"

if [ "$TOTAL_RAM" -gt 0 ]; then
    RAM_USAGE=$((100 * (TOTAL_RAM - AVAILABLE_RAM) / TOTAL_RAM))
else
    RAM_USAGE=0
fi


# -----------------------------
# Collect disk-space information
# -----------------------------

# Check the root filesystem.
# df returns disk usage as a percentage.

DISK_USAGE=$(df -P / | awk 'NR==2 {gsub("%","",$5); print $5}')


# -----------------------------
# Check hard-drive health
# -----------------------------

# We use smartctl when available.
# SMART checks require smartmontools and often root privileges.

DRIVE_STATUS="UNKNOWN"
DRIVE_DEVICE="/dev/sda"

if command -v smartctl >/dev/null 2>&1; then

    # Check whether the device exists before running smartctl.
    if [ -b "$DRIVE_DEVICE" ]; then

        SMART_RESULT=$(sudo smartctl -H "$DRIVE_DEVICE" 2>/dev/null)

        if echo "$SMART_RESULT" | grep -q "PASSED"; then
            DRIVE_STATUS="HEALTHY"
        elif echo "$SMART_RESULT" | grep -q "FAILED"; then
            DRIVE_STATUS="FAILED"
        else
            DRIVE_STATUS="UNKNOWN"
        fi

    else
        DRIVE_STATUS="UNKNOWN"
    fi

else
    # smartctl is not installed.
    DRIVE_STATUS="UNKNOWN"
fi


# -----------------------------
# Compare results against thresholds
# -----------------------------

# CPU status
if [ "$CPU_USAGE" -ge "$CPU_CRITICAL" ]; then
    CPU_STATUS="CRITICAL"
    [ "$OVERALL_STATUS" -lt 2 ] && OVERALL_STATUS=2

elif [ "$CPU_USAGE" -ge "$CPU_WARNING" ]; then
    CPU_STATUS="WARNING"
    [ "$OVERALL_STATUS" -lt 1 ] && OVERALL_STATUS=1

else
    CPU_STATUS="HEALTHY"
fi


# RAM status
if [ "$RAM_USAGE" -ge "$RAM_CRITICAL" ]; then
    RAM_STATUS="CRITICAL"
    [ "$OVERALL_STATUS" -lt 2 ] && OVERALL_STATUS=2

elif [ "$RAM_USAGE" -ge "$RAM_WARNING" ]; then
    RAM_STATUS="WARNING"
    [ "$OVERALL_STATUS" -lt 1 ] && OVERALL_STATUS=1

else
    RAM_STATUS="HEALTHY"
fi


# Disk status
if [ "$DISK_USAGE" -ge "$DISK_CRITICAL" ]; then
    DISK_STATUS="CRITICAL"
    [ "$OVERALL_STATUS" -lt 2 ] && OVERALL_STATUS=2

elif [ "$DISK_USAGE" -ge "$DISK_WARNING" ]; then
    DISK_STATUS="WARNING"
    [ "$OVERALL_STATUS" -lt 1 ] && OVERALL_STATUS=1

else
    DISK_STATUS="HEALTHY"
fi


# Hard-drive status
if [ "$DRIVE_STATUS" = "FAILED" ]; then
    DRIVE_HEALTH_STATUS="CRITICAL"
    [ "$OVERALL_STATUS" -lt 2 ] && OVERALL_STATUS=2

elif [ "$DRIVE_STATUS" = "UNKNOWN" ]; then
    # We treat an inability to verify the drive as WARNING
    # rather than falsely claiming the drive is healthy.
    DRIVE_HEALTH_STATUS="WARNING"
    [ "$OVERALL_STATUS" -lt 1 ] && OVERALL_STATUS=1

else
    DRIVE_HEALTH_STATUS="HEALTHY"
fi


# -----------------------------
# Determine overall server status
# -----------------------------

case "$OVERALL_STATUS" in

    0)
        STATUS_TEXT="HEALTHY"
        ;;

    1)
        STATUS_TEXT="WARNING"
        ;;

    2)
        STATUS_TEXT="CRITICAL"
        ;;

esac


# -----------------------------
# Display health report
# -----------------------------

echo
echo "========================================"
echo "NORTHSTAR SERVER HEALTH CHECK"
echo "========================================"
echo "Server: $SERVER_NAME"
echo "Date: $DATE"
echo

echo "CPU Usage: $CPU_USAGE% [$CPU_STATUS]"
echo "RAM Usage: $RAM_USAGE% [$RAM_STATUS]"
echo "Disk Usage: $DISK_USAGE% [$DISK_STATUS]"

echo
echo "Hard Drive Health:"
echo "$DRIVE_DEVICE: $DRIVE_HEALTH_STATUS"

echo
echo "Overall Status: $STATUS_TEXT"
echo "========================================"


# -----------------------------
# Return appropriate exit code
# -----------------------------

exit "$OVERALL_STATUS"
